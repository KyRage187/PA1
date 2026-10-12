# 04 — ENTSCHEIDUNGEN UND REGELN

> Alles hier ist **vom Bearbeiter entschieden**, nicht von der KI vorgeschlagen. Nicht eigenmächtig ändern.

---

## GETROFFENE ENTSCHEIDUNGEN

| # | Entscheidung | Begründung / Wortlaut |
|---|---|---|
| **D1** | **Die Mengenspalte wird genommen, wie sie dasteht. KEINE Multiplikation mit dem Elternknoten.** | *„wir sagen einfach auch aus der Logik her, das in der steht wie viel man braucht"* |
| **D2** | Nur **Stückgut (`ST`)**. Bei **Menge = 0 bleibt die Zeile erstmal drin.** | *„wenn es 0 stk ist dann behalten wir die erstmal drinne"* |
| **D3** | **Herkunft und Filterung des vorliegenden Exports sind unwichtig.** | *„wir bekommen datensätze die ganz neu sein könnten, das wichtige: der algorithmus muss sitzen"* |
| **D4** | **Kein Scope auf Produktbereiche.** Andere Produktfamilien müssen genauso durchlaufen. | *„der algorithmus soll mit ALLEM funktionieren"* |
| **D5** | Validierung läuft über den betrieblichen Betreuer: wir schicken Funde, er urteilt ja/nein. | – |
| **D6** | **Dummy-Kennzeichen (Sonderbeschaffung `50` und `35`) → raus**, in **beiden** Sonderbeschaffungs-Spalten. Beschaffungs-Schlüssel (Umlagerung/Lohnbearbeitung) → **drin lassen**. | *„wenn 50 dann ist egal welche Spalte, dann fix weg weil dummy"* · *„die sind egal aber lass mal drinne lassen"* |
| **D7** | Pfade im Code **relativ zum Skript** über `pathlib`. | Eigene Änderung des Bearbeiters: `BASIS = Path(__file__).resolve().parent` |
| **D8** | **Der Export stammt aus dem operativen System. Bei Widersprüchen gilt der Export**, nicht das Testsystem. | *„gehe von meiner datei aus, da diese vom operativen system ist, das Testsystem bleibt ein Testsystem"* |

### ⚠️ Anmerkung zu D1
Fachlich ist **noch offen**, ob die Menge sich auf das direkte Überteil oder auf das Fertiggerät bezieht. **D1 ist eine Arbeitsentscheidung, keine geklärte Tatsache.**
Zusätzlich gibt es laut Spaltenreferenz **drei** Mengenfelder, und es ist unklar, welches im Export steht. → siehe `FRAGEN.md`.

---

## CLEANING-REGELN

### ⚠️ ZUERST: die Reihenfolge

Ein Dummy ist eine **Baugruppe mit Kindern**. Wird die Zeile einfach gelöscht, verlieren die Kinder ihr Elternteil.

```
1.  ALLE Zeilen laden, Originalreihenfolge behalten
2.  Baum bauen — UNGEFILTERT
3.  Zeilen zum Entfernen markieren
4.  Kinder entfernter Zeilen an den GROSSVATER umhängen
5.  erst jetzt tatsächlich entfernen
```

**Nicht filtern und dann Baum bauen. Umgekehrt.**
Das entspricht genau der SAP-Option „Auflösen nach Dummystruktur".

> **Belegter Schaden bei falscher Reihenfolge:** Im Rollen-Skript wurden Informationspositionen **vor** dem Baumbau gefiltert → **~5 % der Zeilen** fielen mit „Elternteil fehlt" heraus. Bug ist noch drin.

### Entschiedene Regeln

| # | Regel | Umfang | Status |
|---|---|---|---|
| **R1** | Wiederholte Kopfzeilen raus | ~1.800 Zeilen | ✅ entschieden |
| **R2** | Leerzeilen raus | ~1.800 Zeilen | ✅ entschieden |
| **R3** | Nur Stückgut (`ST`) behalten | von ~117k bleiben ~86k | ✅ D2 |
| **R4** | **Menge = 0 BEHALTEN** — nicht genutzte Alternativposition, **kein Fehler** | ~5.300 Zeilen | ✅ D2 |
| **R5** | Dummy-Kennzeichen `50` raus, in **beiden** Spalten, **Kinder umhängen** | größerer Anteil | ✅ D6 |
| **R6** | Dummy-Kennzeichen `35` raus, Kinder umhängen | unbekannt | ✅ D6 |
| **R7** | Beschaffungs-Schlüssel behalten | – | ✅ D6 |

### Offene Regeln — NICHT eigenmächtig entscheiden

| # | Frage | Anmerkung |
|---|---|---|
| **R8** | Informationspositionen (`P-Typ = X`) raus? | ~9.000 Zeilen. Ich habe das bisher gefiltert — **ist aber nicht entschieden** |
| **R9** | Schüttgut raus? | zwei Felder (Position und Materialstamm). Schüttgut hat keine Einzelbestandsführung → weicht per Definition ab |
| **R10** | Stichtag über das Gültig-ab-Datum? | Stände von 2001 neben 2019 im selben Datensatz |
| **R11** | Alternativpositionen: beide behalten oder nur die mit Einsatzwahrscheinlichkeit > 0? | Im Report gibt es dafür ein Häkchen — **an der Quelle lösbar** |
| **R12** | Echte Dubletten (identische Zeile mehrfach)? | noch nicht geprüft |

---

## DESIGNVORGABEN FÜR JEDEN CODE

Folgen zwingend aus **D3** und **D4** („muss mit allem funktionieren"):

1. **Spalten über NAMEN erkennen, nicht über Position.**
   Alle drei bestehenden Skripte greifen per Index zu (`r[4]` für die Stufe). Bei einem neuen Export sitzt die Spalte woanders.
   → Synonymtabelle je Feld + **Startprotokoll**, welche Spalte als was erkannt wurde. Dann sieht man Fehlzuordnungen sofort.

2. **Keine fest verdrahteten Baureihen, Nummernkreise oder Produktbereiche.**
   Nichts, was bei einem Datensatz aus einem anderen Produktbereich bricht.

3. **Baum zuerst, Filter danach, Kinder umhängen.** Siehe oben.

4. **Bereinigungsprotokoll ausgeben.** Pro Regel: betroffene Zeilen, umgehängte Kinder. Nachvollziehbar — und direkt für die Arbeit verwendbar.

5. **Eine Quelle der Wahrheit.** Nicht jedes Skript liest den Export neu und filtert anders. Bereinigter Datensatz → Datei → alle weiteren Schritte setzen darauf auf.

6. **Die Stufenspalte immer über die ZAHL parsen**, niemals Sternchen zählen (sie wachsen ab Stufe 5/6 nicht mehr mit).

7. **Umlaute reparieren:** `text.encode('gbk').decode('utf-8')` mit Fallback auf `cp1252` und auf „unverändert".

8. **Keine Geschäftsdaten in die Dokumentation.** Ergebnisse in den `.md`-Dateien generisch beschreiben (siehe Vertraulichkeitshinweis in `00_START_HIER.md`). Konkrete Werte gehören in CSV-Ausgaben, nicht in die Doku.

---

## GELERNTE LEKTIONEN — Fallen, die schon zugeschnappt sind

| Falle | Was passierte | Konsequenz |
|---|---|---|
| **„Freundeskreis-Regel"** — Paare transitiv zu Gruppen zusammenfassen (zusammenhängende Komponenten) | **Zweimal** benutzt, **zweimal** über-verschmolzen. Im Rollen-Skript entstand eine Gruppe mit **206 Mitgliedern** aus wenigen falschen Paaren. | Nicht durchketten. Fordern, dass **jeder mit jedem** in der Gruppe ähnlich ist (Cliquen statt Komponenten), oder Schwelle anheben. |
| **Filtern vor Baumbau** | ~5 % der Zeilen verloren | Reihenfolge einhalten |
| **Nullmengen für Anomalien halten** | Fast passiert — hätte **tausende Fehlalarme** erzeugt | Menge 0 = nicht genutzte Alternative, kein Fehler |
| **„Seltenes Teil" als Trigger** | **85 %** aller Geräte geflaggt | Kundenvarianten haben planmäßig Sonderteile. Nur als **Kontext** anzeigen, nicht bewerten. |
| **Code außerhalb des Projektverzeichnisses** | Für den Bearbeiter im Workspace unsichtbar | Immer ins Projektverzeichnis schreiben |
| **Auf einen einzelnen Export hin optimieren** | War meine Priorität — ist falsch | Der Algorithmus ist das Produkt (D3/D4) |
| **Vorgreifen und weiterbauen** | Mehrfach gerügt | Vorschlagen → Go abwarten → machen |
| **Zu viel Text auf einmal** | Mehrfach gerügt | Knapp bleiben, ein Schritt pro Runde |
