# 02 — SAP-/STÜCKLISTEN-FACHWISSEN

> Unternehmensneutral. Keine Material-, Stücklisten- oder Produktbezeichnungen.
> Markierung: **BELEGT** / **VERMUTUNG** / **VOM BEARBEITER BESTÄTIGT** / **OFFEN**
> Alles hier ist SAP-Standardwissen bzw. am System verifizierte Mechanik — keine Geschäftsdaten.

---

## 1. Das Stücklisten-Datenmodell (ECC)

Eine Stückliste liegt nicht in einer Tabelle, sondern in einem Verbund:

```
MAST   Zuordnung:  Material + Werk + Verwendung  →  Stücklistennr. + Alternative
  │
STKO   Kopf:       Basismenge (BMENG), Gültigkeit (DATUV), Status (STLST)
  │
STAS   Auswahl:    welche Position gehört zu welcher Alternative
  │
STPO   Positionen: Komponente (IDNRK), Menge, Positionstyp (POSTP)
```

Objektabhängig ergänzend: `KDST` (Kundenauftrag), `PRST` (Projekt), `EQST` (Equipment), `TPST` (technischer Platz), `DOST` (Dokument). Verwaltung: `STZU`.

**Warum `MAST` separat existiert (BELEGT):** Dieselbe Stückliste wird mehrfach verwendet — eine Stücklistennummer kann in mehreren Werken gelten, einzelne Werke können eigene haben. `MAST` ist die n:m-Zuordnungsschicht; die Inhalte liegen nur **einmal** in `STKO`/`STPO`.

### Zwei Fallen
- **Basismenge `BMENG`:** Positionsmengen gelten **relativ zur Basismenge** des Kopfs. Steht dort 100 und in der Position 5, braucht ein Stück 0,05.
- **Zeitabhängigkeit `DATUV` / Änderungsnummer `AENNR`:** mehrere Stände existieren parallel. Ohne festen Stichtag mischt man Historie mit Gegenwart.

### Verwechslungsgefahr
- **Verwendung `STLAN`** = *für welchen Zweck?* (Fertigung sieht anders als Kalkulation oder Vertrieb)
- **Alternative `STLAL`** = *welcher von mehreren gleichwertigen Wegen?* Gesteuert über die **Produktionsversion** (`MKAL`)

---

## 2. SAP-Standardwerte, die wir brauchen (BELEGT am System)

**Stücklistentypen `STLTY`:** `M` Material · `K` Kundenauftrag · `D` Dokument · `E` Equipment · `S` Standard · `T` technischer Platz · `P` Projekt
Im Projekt relevant: **`M`**.

**Verwendungen `STLAN`** — Texte in `T416T`, Textfeld heißt **`ANTXT`** (nicht `VTEXT`):
`1` Fertigung · `2` Konstruktion · `3` universal · `4` Instandhaltung · `5` Vertrieb · `6` Kalkulation · `7` Leergut · `8` Stabilitätsstudie · `M` Externe Munitionsdarstellung

> ⚠️ **KORREKTUR eines früheren Fehlers:** Ich hatte behauptet, `7`/`8`/`M` seien kundeneigene Verwendungen und deuteten auf bestimmte Branchen. **Falsch — das sind SAP-Standardeinträge.** Nicht so in die Arbeit schreiben.

**Positionstypen `POSTP`** — Texte in `T418T`, Feld `PTEXT`:
`L` Lagerposition · `N` Nichtlagerposition · `R` Rohmaßposition · `T` Textposition · `D` Dokumentposition · `K` Klassenposition · `X` **Informationsposition** · `Z` Hilfsposition · `I` IH-Strukturelement · `C` Planungselement · `M` Intramaterial

> **Nur `L` und `N` sind physische Teile mit Bestandsführung.** `D`, `T`, `X`, `Z` sind Informationsträger und dürfen in einer Mengenanalyse nicht mitgezählt werden.

**Sonderbeschaffung `SOBSL`:** `50` = Phantombaugruppe (Dummy). Weitere Werte codieren Beschaffungswege (Umlagerung, Lohnbearbeitung, Konsignation).

**Backflush-Kennzeichen `MARC-RGEKZ`:** ` ` keine retrograde Entnahme · `1` immer · `2` arbeitsplatzabhängig

**Bewegungsarten:** `261` Warenausgang für Auftrag · `262` Storno · `551`/`552` Verschrottung · `701`/`702` **Inventurdifferenz**

---

## 3. Variantenfertigung — welches Modell vorliegt (BELEGT)

Zwei Wege, Varianten abzubilden:

**Modell A — Variantenkonfiguration (LO-VC):** ein konfigurierbares Material (Materialart `KMAT`), Merkmale und Klassen, **eine** Maximalstückliste, Selektionsbedingungen filtern zur Laufzeit. Konsistenz wird zentral über Beziehungswissen erzwungen.

**Modell B — diskrete Variantenmaterialien:** jede Variante = eigene Materialnummer = eigene Stückliste. Pflege überwiegend durch **Kopieren und Anpassen**. Keine zentrale Logik, keine übergreifende Prüfung.

**Befund:** Im untersuchten System existieren nur **eine Handvoll** konfigurierbarer Materialien gegenüber **Millionen** diskreter Materialien.

> ### → Es liegt eindeutig Modell B vor. Variantenkonfiguration wird praktisch nicht genutzt.

**Und damit ist das Projekt fachlich begründet:**
> Bei Modell B existiert **keine Instanz, die weiß, wie ein Gerät korrekt aussieht.** Jede Variantenstückliste ist ein manuell gepflegtes Einzelstück, meist kopiert. Ein Kopierfehler hat keinerlei Gegenprüfung. Genau diese fehlende Wahrheit soll statistisch aus der Masse rekonstruiert werden.

**Nebeneffekt, in den Daten sichtbar:** historisch gewachsene, **nicht analog aufgebaute Nummernkreise** — Altbestände, Zukäufe, verschiedene Werksbestände und migrierte Vorsysteme nebeneinander. Deshalb trägt die Materialnummer bei **Bauteilen** keine verlässliche Semantik.

---

## 4. Retrograde Entnahme — DER zentrale Mechanismus

**Vom Bearbeiter als zentral markiert** — kam im Vortragsfeedback sehr häufig vor. Nicht als Randnotiz behandeln.

```
fehlerhafte Stückliste (z. B. ein Bauteil doppelt)
        ↓
Fertigungsauftrag anlegen  →  Stücklistenauflösung
        ↓
Soll wird als Reservierung im Auftrag eingefroren
        ↓
Rückmeldung des Vorgangs
        ↓
retrograde Entnahme bucht Warenausgang (261) AUTOMATISCH
   → gebucht wird, was in der Stückliste STEHT, nicht was verbaut WURDE
        ↓
Bestand SAP ≠ Bestand real  →  Inventurdifferenz, Materialüberverbrauch
```

> **Merksatz: Bei retrograder Entnahme ist die Stückliste nicht Beschreibung, sondern Buchungsvorschrift.**

**Warum es weh tut:** Ohne Backflush würde im Lager auffallen, dass ein Teil übrig bleibt. Mit Backflush passiert die Fehlbuchung **vollautomatisch und unbemerkt**.

**BELEGT:** Bei **rund der Hälfte** aller Material/Werk-Kombinationen im untersuchten System ist Backflush aktiv (`MARC-RGEKZ` gesetzt). Regelfall, kein Randphänomen.

### Für einen Soll/Ist-Vergleich relevante Objekte
| Objekt | Rolle |
|---|---|
| **Reservierungstabelle** (`RESB`) | die **im Auftrag eingefrorene aufgelöste Stückliste** → das **SOLL** |
| **Auftragsbewegungen** (`AUFM`) | Warenbewegungen je Fertigungsauftrag → das **IST** |
| Materialbelege (`MSEG` / `MKPF`) | Bewegungspositionen und -köpfe |
| Auftragskopf/-position (`AFKO` / `AFPO`) | Auftragskontext |
| Materialbelegliste (Transaktion) | Einstieg für Menschen |

> **Wichtige Einsicht:** Korrekturbuchungen sind der **Beweis**, dass eine Stückliste falsch ist. **Systematische** Abweichung über viele Aufträge = Stammdatenfehler. Einmalige Abweichung = Rauschen. **Das wären die fehlenden Labels** — aber es gibt keinen zuverlässigen Zugriff (siehe `00_START_HIER.md`).

---

## 5. Alternativpositionen — vollständig geklärt (BELEGT)

Die wichtigste Klärung des Tages. Vier Felder in `STPO`:

| Datenelement | Bedeutung |
|---|---|
| `CS_ALPGR` | **Alternativposition: Gruppierung** — welche Teile sind Alternativen zueinander |
| `CS_ALPRF` | **Alternativposition: Rangfolge** — erste, zweite Wahl … |
| `CS_ALPST` | **Alternativposition: Strategie** — Werte `1` und `2`, **Bedeutung OFFEN** |
| `EWAHR` | **Alternativposition: Einsatzwahrscheinlichkeit in %** |

### Die Regeln (am System an einem Bauteilpaar verifiziert)

**Regel 1 — `EWAHR` summiert sich innerhalb einer `ALPGR`-Gruppe einer Stückliste auf 100.**

**Regel 2 — gewichtete Menge = `Menge × EWAHR / 100`**

Schematisch, eine Stückliste, eine Position, eine Alternativgruppe:

| Alternative | Menge | `ALPRF` | `EWAHR` | → gewichtete Menge |
|---|---|---|---|---|
| Variante 1 | 4 | 01 | **0** | **0** |
| Variante 2 | 4 | 02 | **100** | **4** |

### → Menge 0 ist KEIN Fehler
> Eine Position mit `EWAHR = 0` ist eine **dokumentierte, aber aktuell nicht genutzte Alternative**. Sie erzeugt keinen geplanten Bedarf.

**Eine Falle, in die wir fast gelaufen wären:** Hätten wir die Nullmengen als „auffällig" geflaggt, wären **tausende Fehlalarme** entstanden — mit dem sicheren Gefühl, etwas gefunden zu haben.

### Regel 3 — es ist eine Eigenschaft der POSITION, nicht des Materials (BELEGT)

Dasselbe Bauteilpaar hatte in **verschiedenen** Stücklisten die Werte **vertauscht**: mal war die erste Wahl aktiv, mal die zweite.

> **Man kann also nicht sagen „Bauteil X hat immer 0".** Die Einsatzwahrscheinlichkeit gehört zur Stücklistenposition.

Außerdem gibt es **mehrere Alternativgruppen pro Stückliste** — beobachtet, Bedeutung offen.

### Auffälliges Muster (OFFEN)
In mehreren Fällen stand die **erste Wahl auf 0 %** und die **zweite auf 100 %**. Möglicherweise wird die erste Wahl nicht mehr benutzt. → Frage an die Fachseite.

### Konsequenz für das Projektziel
> **Alternativpositionen sind eine strukturelle Quelle für „gebucht ≠ Tabelle".**
> Die Stückliste listet mehrere Möglichkeiten. Geplant und retrograd gebucht wird nur die mit `EWAHR = 100`. Wird in der Fertigung eine andere verbaut, weicht die Buchung **zwangsläufig** ab — ohne dass jemand einen Fehler gemacht hat.

Vermutlich ein Grund, warum das Thema im Feedback so präsent war.

---

## 6. Sonderbeschaffung und Dummys — VOM BEARBEITER BESTÄTIGT

**Es gibt ZWEI Sonderbeschaffungs-Felder** in der Report-Ausgabe:
```
Sonderbeschaffungsart der STÜCKLISTENPOSITION   (Datenelement CS_SOBSL)
Sonderbeschaffungsart aus dem MATERIALSTAMM     (Datenelement SOBSL)
```
Analog gibt es **Schüttgut zweimal** — einmal aus der Position, einmal aus dem Materialstamm.

### Die Regeln des Bearbeiters
| Wert | Bedeutung | Regel |
|---|---|---|
| **50** | Dummy / Phantombaugruppe | **raus** — *„wenn 50 dann ist egal welche Spalte, dann fix weg weil dummy"* |
| **35** | ebenfalls Dummy | **raus** |
| Beschaffungs-Schlüssel (Umlagerung, Lohnbearbeitung) | Lieferant sorgt für Lieferung | **drin lassen** |

Begründung: *„nur für Beschaffung, nicht für die Interpretation der BOM"* → diese Werte sind harmloses Rauschen.

### ⚠️ Kritisch: die Reihenfolge beim Entfernen
Ein Dummy ist eine **Baugruppe mit Kindern**. Wird die Zeile einfach gelöscht, verlieren die Kinder ihr Elternteil.

**Richtig ist: Kinder hängen sich an den Großvater.** Genau das macht SAP mit der Option „Auflösen nach Dummystruktur".

```
1.  ALLE Zeilen laden, Originalreihenfolge behalten
2.  Baum bauen — UNGEFILTERT
3.  Zeilen zum Entfernen markieren
4.  Kinder entfernter Zeilen an den GROSSVATER umhängen
5.  erst jetzt tatsächlich entfernen
```
**Nicht filtern und dann Baum bauen. Umgekehrt.**

> **Belegter Schaden bei falscher Reihenfolge:** Im Rollen-Skript wurden Informationspositionen **vor** dem Baumbau gefiltert → **rund 5 % der Zeilen** fielen mit „Elternteil fehlt" heraus. Bug ist noch drin.

---

## 7. Baugruppen aus einer flachen Auflösungsliste rekonstruieren

> ⚠️ **VOM BEARBEITER KORRIGIERT:** Ich hatte behauptet „Baugruppen sind eindeutig ableitbar". **Zurückgezogen.** Er hat das so im Haus erklärt und wurde **schief angeschaut**. Technisch möglich ist eine Eltern-Kind-Rekonstruktion. Ob das fachlich einer **Baugruppe** entspricht, ist **OFFEN** und steht als Frage an die Fachseite.

### Die technische Regel
> Das Elternteil einer Zeile ist die nächste Zeile darüber, die **eine Stufe weniger** hat.

Schematisch:
```
Stufe 1       Baugruppe A            ← Kind des Erzeugnisses
Stufe   *2      Bauteil a1           ← Kind von A
Stufe   *2      Bauteil a2           ← Kind von A, GESCHWISTER von a1
Stufe   *2      Baugruppe B          ← Kind von A
Stufe     **3     Bauteil b1         ← Kind von B  (nicht von a1 oder a2!)
Stufe     **3     Bauteil b2         ← Kind von B
```
Umsetzung: ein Stapel, der je Stufe das zuletzt gesehene Teil merkt; tiefere Einträge werden beim Aufsteigen verworfen.

**Es werden nur direkte Kinder verwendet, nicht der ganze Unterbaum** — Geschwister sind die Teile, die in **einem Arbeitsschritt** zusammengefügt werden. Das ist das scharfe Signal; der ganze Unterbaum verwässert (ein tief liegendes Rohmaterial wäre sonst „Nachbar" eines Verpackungsetiketts).

### Zwei unabhängige Bestätigungen der Struktur
1. **Die Positionsnummer springt bei jeder neuen Baugruppe auf 1 zurück** — sie ist die Position *innerhalb der jeweiligen Stückliste*. Gut in Excel nachprüfbar: wo die Positionsnummer zurückspringt, fängt eine neue Baugruppe an.
2. Die Stufenwerte selbst.

### ⚠️ Die Struktur hängt an der Zeilenreihenfolge
Die Stufe allein sagt nur „ich bin auf Ebene 3". *Unter wem* ergibt sich erst aus der Zeile darüber.
> **Wer das Blatt in Excel sortiert, zerstört die Struktur unwiderruflich.** Filtern ist ungefährlich, Sortieren nicht.

### Die Stufenspalte richtig parsen
Die Werte sind mit Sternchen eingerückt (`1`, `*2`, `**3`, …). **Die Sternchen wachsen ab Stufe 5/6 nicht mehr mit** — mehrere Stufen haben dieselbe Anzahl. Das zugehörige Datenelement heißt „Auflösungsstufe (**aufbereitet**)", die Einrückung ist also bewusst kosmetisch.
> **Immer die ZAHL auswerten, niemals Sternchen zählen.**

---

## 8. Das Rollen-Konzept

Der methodische Kern des bisherigen Ansatzes.

> **Eine Rolle ist eine funktionale Position im Gerät. Eine Rollenbesetzung ist das konkrete Teil, das diese Position in dieser Variante füllt.**

### Wie Rollen ohne Vorwissen erkannt werden
```
gleiche Nachbarschaft  +  kommen ZUSAMMEN vor   →  ergänzen sich
gleiche Nachbarschaft  +  kommen NIE zusammen   →  ERSETZEN sich  →  gleiche Rolle
```
**Beide Bedingungen sind nötig.** Ohne die zweite findet man Teile, die zusammen **gehören**, statt Teile, die sich **ersetzen**.

Das Prinzip entspricht der Verteilungshypothese aus der Sprachverarbeitung: Bedeutung ergibt sich aus dem Kontext. Hier: Teile statt Wörter, Baugruppen statt Sätze.

> ⚠️ **VOM BEARBEITER KORRIGIERT:** Nicht „das müssen die **gleichen Bauteile** sein", sondern „Bauteile mit der **gleichen Aufgabe**". Zwei Spannungsvarianten einer Baugruppe sind **nicht dasselbe Teil** — sie besetzen dieselbe Stelle im Bauplan. Wer sie für identisch hält, kann die bekannten Abweichungsfälle nie finden.

### Warum das Konzept alles ordnet
| Beobachtung | In Rollen ausgedrückt |
|---|---|
| Bauteil doppelt eingetragen | Rolle **doppelt** besetzt |
| Pflichtbauteil fehlt | Rolle **unbesetzt** |
| falsche Spannungsvariante verbaut | Rolle mit **falscher Variante** besetzt |
| Kundenvariante nutzt anderes Bauteil | Rolle **legitim anders** besetzt → kein Fehler |
| Nummernkreise nicht analog | irrelevant, weil Rolle ≠ Nummer |

### Aktuelle Funktion im Projekt: WERKZEUG, nicht Ziel
Nachdem das Ziel auf Buchungsabweichungen geschärft wurde, dient das Rollenmodell dazu, **Vergleichbarkeit herzustellen**: zwei unterschiedlich benannte Kurztexte für dieselbe Funktion sind zwei Strings, aber eine Sache. Ohne Rollen zählt man falsch.

### Bekannte Schwäche: Substitution wird nicht erkannt
Der Code sieht „Pflichtteil fehlt" und „Sonderteil ist da" als **zwei getrennte Fakten** und kann sie nicht verbinden. Deshalb waren **alle** Kandidaten aus Ansatz 1 als „evtl. Substitution" markiert — die Heuristik trennt nichts.

Lösungsoptionen:
1. Das Klassifizierungsmerkmal zur Bauteilart nutzen — nur zu ~19 % gefüllt
2. Wortstämme/Synonyme — wäre die unerwünschte Regelliste
3. Textähnlichkeit / Embeddings — der einzige Punkt, an dem ein Sprachmodell wirklich gebraucht wird

---

## 9. Nummernsystematik — VERMUTUNGEN, ausdrücklich nicht belegt

Der Bearbeiter sagt, man kann **teils an der Materialnummer ablesen, was für ein Teil es ist**. Beobachtungen, die das stützen (ohne konkrete Nummern):

1. **Ein Buchstabe im hinteren Teil der Materialnummer scheint die Objektart zu codieren** — Einzelteil / Baugruppe / Druckvorlage / Dokument. Baugruppen tragen im Kurztext meist einen „komplett"-Zusatz.
2. **Ein bestimmter Zahlensuffix scheint reserviert** für „Basisteile"-Sätze — beobachtet über **mehrere verschiedene Nummernstämme hinweg**, jedes Mal mit derselben Bedeutung.
3. **Mittlere Ziffernblöcke scheinen Teileklassen zu codieren** — z. B. Kunststoffteile, Feder-/Magnetteile, Stanzteile, Baugruppen jeweils in eigenen Blöcken.

### ⚠️ Wichtig für die Umsetzung
**Nicht hart in Code schreiben.** Das wäre wieder ein Parser und widerspricht der Vorgabe „muss mit allem funktionieren" — und in einem anderen Produktbereich gilt die Systematik vermutlich anders.

**Sondern: die Zuordnung aus den Bezeichnungen lernen und messen, wie zuverlässig sie ist.** Dann lautet das Ergebnis: *„Der Nummernstamm erlaubt in Bereich X eine Klassifikation mit N % Trefferquote"* — ein **Befund** statt einer Annahme. Das ist zugleich ein vorzeigbares Teilergebnis für die Arbeit.

### Geräte vs. Bauteile — wichtige Unterscheidung
Bei **Erzeugnissen** sieht der Nummernstamm systematisch aus: Spannungsvarianten derselben Baureihe teilen den Stamm und unterscheiden sich nur im Suffix.

**ABER Gegenbeispiel gefunden:** Zwei Erzeugnisse mit **demselben Stamm** waren **verschiedene Typen** (unterschiedliche Kontaktkonfiguration). Das Suffix codiert also **mehrere Merkmale gleichzeitig**.

> ⚠️ Die Notiz „Nummernkreise nicht analog aufgebaut" gilt sicher für **Bauteile**, bei **Geräten** ist sie nicht bestätigt. Meine Erklärung dazu **ist nicht angekommen** — muss neu und einfacher erklärt werden (steht in `FRAGEN.md`).

---

## 10. Was SAP prüft und was nicht

| SAP prüft (formal/technisch) | SAP prüft **nicht** (semantisch) |
|---|---|
| Existiert das Material? | Ist die Menge plausibel? |
| Ist die Mengeneinheit umrechenbar? | Passt die Anzahl zum Gerät? |
| Ist der Positionstyp erlaubt? | Fehlt eine Pflichtkomponente? |
| Rekursion (Material in sich selbst)? | Weicht die Stückliste von ähnlichen Geräten ab? |
| Status / Gültigkeit gesetzt? | Ist die Struktur fachlich sinnvoll? |

**Für die Arbeit präzise formulieren:**
> SAP validiert Stücklisten **syntaktisch und referenziell, aber nicht semantisch.** Es prüft Wohlgeformtheit, nicht Plausibilität.

Die pauschale Aussage „SAP validiert Stücklisten kaum" ist angreifbar — ein Gutachter greift da rein.
