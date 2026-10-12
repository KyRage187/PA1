# 05 — CODE-INVENTAR

> Unternehmensneutral. Ergebnisse sind **generisch** beschrieben — konkrete Materialnummern und
> Bezeichnungen stehen in den CSV-Ausgaben, nicht in dieser Doku.

Drei Codestände im Projektverzeichnis. **Sie belegen die Entwicklung des Tages und werden bewusst behalten** — die zwei gescheiterten Ansätze sind Teil des Ergebnisses.

---

## Skript 1 — Anomaliesuche `bom_pattern.py`
**Status: GEPARKT.** Nicht weiterentwickeln, als Belegstück behalten.

**Was es macht:** Geräte über den Kurztext in Familien gruppieren → pro Familie lernen, welches Teil in wie viel Prozent der Geschwister vorkommt und in welcher Menge → Abweichungen gewichten → Kandidatenliste mit Evidenz.

**Drei Prüfungen:**
- Pflichtteil fehlt (Teil in ≥ 90 % der Geschwister, hier nicht)
- Menge unüblich (diese Menge kommt bei < 10 % vor)
- seltenes Teil (Teil in ≤ 15 % der Geschwister)

**Ergebnis:**
```
~105.000  verwendbare Positionszeilen
   1.466  Geräte in auswertbaren Familien (115 Familien mit ≥ 5 Mitgliedern)
   1.246  Kandidaten  →  85 %  →  UNBRAUCHBAR
```
Nach Abschalten des Signals „seltenes Teil":
```
      18  Kandidaten (1,2 %)
      26  Befunde „Pflichtteil fehlt"
       2  Befunde „Menge unüblich"
```

**Der beste Treffer, generisch:** Ein Gehäuse-Bauteil war mit **Menge 2** eingetragen, während **28 von 29** vergleichbaren Geräten dort Menge 1 haben. Genau die Fehlerklasse, die der Bearbeiter ursprünglich beschrieben hatte.
Zweitbester: ein Verbindungsteil mit deutlich erhöhter Menge gegenüber dem Familienwert, zusammen mit mehreren fehlenden Pflichtteilen.

**Bekannte Schwäche:** **Alle 18** Kandidaten sind als „evtl. Substitution" markiert → die Heuristik trennt nichts. Sie prüft nur, *ob* ein Sonderteil vorhanden ist, nicht *ob es das fehlende ersetzt*.

**Ausgabe:** `kandidaten.csv`

---

## Skript 2 — Gerätegruppierung `gruppierung.py`
**Status: BRAUCHBAR.** Liefert die Absicherung für „Töpfe nicht vermischen" (Phase 2).

**Was es macht:** Drei bewusst **unabhängige** Gruppierungsverfahren und deren Vergleich.

| Weg | Grundlage | Annahme |
|---|---|---|
| **A** | erstes Wort des Materialkurztexts | Benennung ist systematisch |
| **B** | Stamm der Materialnummer, Variantensuffix abgeschnitten | Nummernsystematik ist systematisch |
| **C** | Jaccard-Ähnlichkeit der Teilemengen, Schwelle 0,80 | **keine** |

**Ergebnis:**
| | Gruppen | mit ≥ 5 | Einzelgänger | größte |
|---|---|---|---|---|
| A Kurztext | **271** | 115 (1.466 Geräte) | 113 | 29 |
| B Nummernstamm | **87** | – | – | – |
| C Struktur | **1.537** | – | – | – |

**Übereinstimmung:**
```
A → B :  97,7 %      B → A :  64,1 %
A → C :  29,9 %      C → A :  60,2 %
B → C :  21,6 %      C → B :  75,0 %
alle drei identisch:  5 von 1.692 (0,3 %)
```

### Die zwei Kernaussagen

**1. A und B bestätigen sich gegenseitig und bilden eine zweistufige Hierarchie.**
Hohe Reinheit A→B, niedrige B→A bedeutet: jede A-Gruppe liegt **komplett in einer** B-Gruppe, jede B-Gruppe enthält **mehrere** A-Gruppen.
```
B  Nummernstamm  =  gröbere Ebene   ( 87 Gruppen)  ≈ Baureihe
A  Kurztext      =  feinere Ebene   (271 Gruppen)  ≈ Typ
```
**Analogie zum Erklären: Städte und Länder.** Jede Stadt liegt in genau einem Land (100 % rein), jedes Land enthält viele Städte (niedrige Reinheit). **Aus der Schieflage liest man ab, welche Ebene die feinere ist** — ohne dass es jemand sagt.

Kurztext (von Menschen getippt) und Materialnummer (Systematik) sind **unabhängige** Informationen. Dass sie zu 97,7 % dasselbe sagen, ist **echte Kreuzvalidierung** der Gruppierung.

**2. Weg C ist nachweisbar gescheitert — und der Grund ist verstanden.**
```
Teile pro Gerät: min 1, max 14, Durchschnitt 4,8
```
Bei 5 Teilen und **einem** Unterschied: `4/6 = 0,67` → unter der Schwelle 0,80.
Bei 50 Teilen und **einem** Unterschied: `49/51 = 0,96` → deutlich drüber.

> **Bei fünf Teilen ist jeder einzelne Unterschied ein Erdbeben, bei fünfzig ein Rauschen.**

**Ursache:** Die Konfiguration nutzt nur **Stufe 1** und wirft damit ~92 % der Zeilen weg.
**Fix (nie ausprobiert):** Konfiguration auf „alle Stufen" umstellen — **eine Zeile Änderung.** Der Schalter wurde bewusst nicht gedreht, weil das Ziel sich vorher verschoben hat.

**Ausgabe:** `produktgruppen.csv` (Gerät → Gruppe je Verfahren), Laufprotokoll als Textdatei

**Offener Punkt:** Läuft global über alle Geräte. Für „Töpfe nicht vermischen" müsste es **je Baureihe** laufen.

---

## Skript 3 — Rollenerkennung `rollen.py`
**Status: BRAUCHBAR, jetzt als WERKZEUG** (nicht mehr als Ziel).

**Was es macht:**
1. Baum aus Stufenspalte + Zeilenreihenfolge rekonstruieren → **2.530** Eltern-Kind-Einheiten
2. Pro Teil den Kontext bilden: mit welchen Teilen wurde es je in einer Einheit verbaut
3. **Häufige Nachbarn abschwächen** (IDF-Gewichtung) — das häufigste Teil steckt in über 1.600 Einheiten und sagt nichts aus. Analogie: Stoppwörter im Text.
4. Kosinus-Ähnlichkeit der gewichteten Kontexte
5. **Rollenpaar = ähnlicher Kontext (≥ 0,70) UND so gut wie nie gemeinsam verbaut (≤ 2 %)**
6. Paare zu Gruppen zusammenfassen

**Ergebnis:**
```
  2.530  Eltern-Kind-Einheiten
    986  verschiedene Teile
    558  davon mit ≥ 3 Vorkommen (auswertbar)
 11.558  Rollenpaare
     24  Rollengruppen  →  etwa 15 fachlich klar richtig
```

### Die überzeugenden Treffer, generisch beschrieben

| Befund | Warum er zählt |
|---|---|
| **Acht Ausführungen eines Drahtmaterials in verschiedenen Stärken, verteilt über ZWEI verschiedene Nummernkreise** | **Das Nummernkreis-Problem in Reinform gelöst.** Über die Materialnummer nicht als zusammengehörig erkennbar, über den Verbauzusammenhang eindeutig. |
| **Ein Formteil in drei Farbausführungen** | Genau der **Substitutionsfall**, an dem Ansatz 1 gescheitert ist — vorher „Teil fehlt!", jetzt „eine Rolle, drei Besetzungen". |
| **50 Besetzungen einer spannungsabhängigen Baugruppe — unter ZWEI verschiedenen Bezeichnungskonventionen** | Der **Variantenträger**. Und **genau die Rolle, um die sich alle bekannten Abweichungsfälle drehen** — das Skript hat den relevanten Bereich von selbst gefunden. |
| **83 bzw. 64 Spannungsvarianten je einer montierten Baugruppe** | Große, sauber abgegrenzte Rollen. |
| **Kontaktbestückungen in ZWEI getrennte Rollen aufgeteilt** | Die Trennung ist fachlich sinnvoll — das Verfahren übergeneralisiert hier nicht. |
| **Fünf „Basisteile"-Sätze verwandter Ausführungen** | Bestätigt die Vermutung zum reservierten Nummernsuffix. |

### Die Fehler
- **Eine Gruppe mit 206 Mitgliedern ist Müll.** Sie beginnt mit einem Formteil und schluckt dann über 200 Varianten einer montierten Baugruppe. **Ursache: Durchketten von Paaren.** Ein einziges falsches Paar verklebt zwei Rollen.
- **Eine Gruppe mischt Bauteilarten** — drei Varianten desselben Teils plus zwei fremde Teile aus derselben Baugruppen-Region. Das gemeinsame Wort im Kurztext verrät den Fehler.
- **BUG: ~5 % der Zeilen fielen mit „Elternteil fehlt" heraus** — Informationspositionen wurden **vor** dem Baumbau gefiltert, dadurch reißen Ketten. **Noch nicht behoben.**

**Ausgabe:** `rollen.csv` mit den Spalten `Rolle_Nr`, `Anzahl_Besetzungen`, `Kontextaehnlichkeit`, `Identnummer_Teil`, `Materialkurztext_Teil`, `Vorkommen_in_Baugruppen` · Laufprotokoll als Textdatei

---

## Wie die Rollen validiert werden (abgesprochen, noch nicht gemacht)

Es gibt **keine Wahrheitstabelle** — niemand hat eine Liste rollengleicher Teile. Wenn es die gäbe, bräuchte man das Projekt nicht. Also **mehrere unabhängige Prüfungen**:

| # | Prüfung | Wer |
|---|---|---|
| 1 | **Augenscheinvalidität** — Ausgabe nach Rollennummer sortieren, pro Rolle ja/nein/unklar urteilen → ergibt eine Trefferquote als Zahl | **Bearbeiter** |
| 2 | **Die Bedingung selbst nachprüfen** — zwei Teile einer Rolle im Export filtern, Pivot mit dem Erzeugnis in den Zeilen: erscheint ein Erzeugnis mit **beiden**? Wenn nein → Bedingung bestätigt. **Harter Test, im Meeting vorführbar.** | **Bearbeiter, in Excel** |
| 3 | **Die Bezeichnung als unabhängiger Zeuge** — der Code hat den **Wortinhalt nie zur Gruppierung benutzt**, nur als Namensschild. Teilen die Mitglieder einer Rolle ein Schlüsselwort, ist das **echte, unabhängige Bestätigung**. Umgekehrt verrät ein nur oberflächlich gemeinsames Wort einen Fehlertreffer. | rechenbar |
| 4 | **Domänenexperte** — 10 Rollen ausdrucken, **eine** Frage: „Sind das austauschbare Ausführungen desselben Bauteils?" Fünf Minuten, zehn Antworten. | Montage |
| 5 | **Abdeckung** — wie viele Teile haben überhaupt eine Rolle bekommen | rechenbar |

Prüfung 1, 2 und 4 macht der **Bearbeiter selbst** — das Argument ist stärker, wenn er es gesehen hat.

---

## Lokale Ausführung (Inhalt der verschwundenen Anleitung)

```
1.  python --version           (unter Windows evtl. py --version)
2.  pip install openpyxl       (bei PEP-668: --user  oder  --break-system-packages)
3.  Skript + Export in EINEN Ordner legen
4.  cd <Ordner>  →  python <skript>.py
```

**Drei Stolperfallen:**
1. **Windows-Pfade brauchen ein `r` vor dem String**, sonst werden Backslashes als Sonderzeichen gelesen.
2. **Der Blattname endet auf ein Leerzeichen.** Nicht entfernen, sonst `KeyError`.
3. **`PermissionError` beim CSV-Schreiben** heißt: die CSV ist noch in Excel offen.

Der Bearbeiter hat lokal auf relative Pfade via `pathlib` umgestellt (D7) — **seine Version ist aktueller als die im Container.**

Das Skript öffnet die Arbeitsmappe selbst; Excel muss nicht laufen.
Im Container installiert: `openpyxl`, `pandas`, `pyxlsb`. **Ladezeit ~0,2 s** — Performance ist nie das Problem.

---

## Was NICHT existiert und gebraucht wird

- **Ein Bereinigungsskript** nach den Regeln aus `04` — mit Baum-zuerst-Reihenfolge, Kinder-Umhängen, **Spaltenerkennung über Namen** und Bereinigungsprotokoll.
  **Das ist der nächste Codeschritt**, sobald die Export-Frage entschieden ist.
- Prüfung 3 und 5 der Rollen-Validierung
- Rollenerkennung **je Baureihe** statt global
- Behebung des „Elternteil fehlt"-Bugs
- Der Test „alle Stufen" in Skript 2 (eine Zeile)
