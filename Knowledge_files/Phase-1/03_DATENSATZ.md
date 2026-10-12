# 03 — DER DATENSATZ

> Unternehmensneutral. Keine Datei-, Blatt-, Produkt-, Material- oder Kundenbezeichnungen.
> Aggregierte Kennzahlen sind enthalten, weil sie die methodischen Schlüsse tragen.

---

## Herkunft und Rahmen

- **Ein Stücklisten-Export als Excel-Arbeitsmappe**, ca. 14 MB, **Exportstand 2019**
- Ursprünglich als **Excel-Binärformat** (`.xlsb`) erhalten → das Upload-Format wurde nicht akzeptiert → in `.xlsx` umgewandelt
- **`.xlsx` ist das richtige Arbeitsformat.** Lädt in ~0,2 s mit `openpyxl(read_only=True)` — Größe ist nie das Problem
- **⚠️ Die Datei stammt aus dem OPERATIVEN System**, nicht aus einem Testsystem
  → **Bei Widersprüchen zwischen Export und Testsystem gilt der Export**
  → und: es sind **Produktivdaten**, entsprechend behandeln

## Erzeugt mit

Einem **kundeneigenen Report zur Stücklistenauflösung für Fertigungsstücklisten** (`<Z-REPORT>`).

- Gewachsen seit **1999**, letzte Änderungen **Ende 2025**
- Kommentar im Quellcode: *„wird in der gesamten Montage genutzt"* → **„Mont" in den Notizen des Bearbeiters = Montage**
- **Im Dialog auf 20 Stücklisten begrenzt**, darüber ist Hintergrundeinplanung nötig → daher die Notiz „im Hintergrund laufen lassen"

**Wesentliche Selektionsparameter:**
| Parameter | Default |
|---|---|
| Materialien | – |
| Werk | vorbelegt mit dem Produktionsstandort |
| Auflösungstiefe | **99** = alle Ebenen |
| **Stichtag** | Systemdatum |
| Verwendung | **3 = universal** |
| Alternative | **01** |
| Fertigungsversion | 1–99 |
| Einsatzmenge | 1 |

**Die Selektionsvariante** wurde vom Bearbeiter zunächst mit einem Tippfehler notiert (ein Buchstabe falsch). Der korrekte Name steht im System.

---

## Struktur der Arbeitsmappe

Sieben Blätter:

| Blatt | Größe | Status |
|---|---|---|
| **Arbeitsblatt** (die aufgelöste Stückliste) | 117.265 × 27 | **hier passiert alles** |
| **Blatt mit geprüften Abweichungsfällen** | 19 × 8 | **wichtig, siehe unten** |
| Selektionsblatt | 9.748 × 14 | **nie ausgewertet** |
| Erzeugnisliste | 2.303 × 27 | **nie ausgewertet** |
| Änderungsblatt 2017–2019 | 636 × 8 | **nie ausgewertet** |
| zwei Restblätter | klein | ein Beispielgerät + Reste |

> ⚠️ **Der Name des Arbeitsblatts endet auf ein Leerzeichen.** Wer es im Code entfernt, bekommt `KeyError`. Nicht „korrigieren".

---

## Kennzahlen des Arbeitsblatts

```
117.264  Datenzeilen
113.677  echte Datenzeilen
  1.821  WIEDERHOLTE KOPFZEILEN mitten in den Daten
  1.766  Leerzeilen
  1.692  Erzeugnisse
  ~1.400 verschiedene Komponenten
  2.530  Eltern-Kind-Einheiten rekonstruierbar (Ø 4,2 Kinder, min 1, max 14)
```

### Verteilungen (vor Bereinigung)

**Auflösungsstufe:** Stufe 1 → ~12.000 Zeilen, Stufen 2–5 jeweils **15.000–23.000**, Stufen 6–8 abnehmend bis ~4.800.
→ **Stufe 1 ist nur ~11 % der Daten.** Wer nur Stufe 1 nimmt, ignoriert 89 %.

**Mengeneinheiten:** `ST` ~86.000 · Liter ~15.000 · `KG` ~4.500 · Gramm ~4.200 · Meter ~2.400 · Millimeter ~1.800 · Rest vernachlässigbar
→ Neben Stückgut gibt es relevante Mengen an Flüssigkeiten und Gewichtsmaterial (Verbrauchsstoffe).

**Positionstypen:** `L` ~105.000 · `X` ~9.000 — **nur diese zwei**

**Mengen:** `1` ~57.000 · `2` ~17.000 · `0,001` ~12.000 · `4` ~5.900 · **`0` ~5.300** · `8` ~2.100

**Gültig-ab-Datum:** Stände von **2001 bis 2019** parallel im Datensatz. Häufigste Einzelstände umfassen jeweils 10.000–32.000 Zeilen.

**Klassifizierungsmerkmal zur Bauteilart:** nur **19 % gefüllt**. Enthält generische Bauteilkategorien (Gehäuseteile, Verpackung, Kontaktteile, Isolierteile, Hilfs-/Betriebsmittel …). **Wäre der billigste Weg zur Substitutionserkennung — ist aber zu lückenhaft.**

---

## Produktinhalt (bewusst unspezifisch)

- Der Export enthält **eine Produktdomäne**, nicht das gesamte Portfolio. Andere Produktbereiche sind **nicht** enthalten.
- Mehrere verwandte Baureihen mit jeweils **20–30 Varianten**
- **Die Varianten unterscheiden sich fast durchgängig über eine elektrische Kenngröße** (Spannung) → **der Variantenträger ist eine bestimmte Baugruppe**
- **Kundenspezifische Varianten** kommen vor und sind am Kurztext erkennbar (Kundenname oder Kundenteilenummer im Text). Sie haben **planmäßig** Sonderteile — Sondergehäuse, Sonderverpackung, Kundenetikett.
  → **Das ist kein Fehler.** Und es war die Ursache der 85-%-Fehlalarmquote von Ansatz 1.
- Beobachtet wurde außerdem ein **Tippfehler in einem Materialkurztext** (fehlender Anfangsbuchstabe). Belegt: Kurztexte sind **nicht fehlerfrei gepflegt** → exakter Textvergleich ist zu spröde, es braucht Toleranz.

---

## Das Blatt mit den geprüften Abweichungsfällen — die einzige Wahrheitsquelle

19 Zeilen, offenbar **von Hand geprüft**. Struktur: Vergleich zwischen **„welche Komponente sagt die Referenzquelle"** und **„welche Komponente steht in der Stückliste"**.

**Das Fehlermuster (generisch beschrieben):**
1. Die elektrische Kenngröße im **Gerätenamen** passt nicht zur Kenngröße in der **Komponentenbezeichnung**
2. **Strom-Art verwechselt** (Wechsel- statt Gleichspannung) bei ansonsten gleicher Kenngröße
3. Die erwartete Komponente ist in der Stückliste **gar nicht gefunden**

### Warum das Blatt wichtig ist
- **Die einzigen bestätigten Fehlerfälle**, die existieren → Validierungsmenge
- **Alle vom selben Typ** → deckt nicht alle Fehlerarten ab
- Die Fälle liegen auf **Stufe 2/3**, nicht auf Stufe 1 → **eine Stufe-1-Analyse kann sie prinzipiell nicht finden**
- Sie belegen den Punkt des Bearbeiters: die Rolle ist besetzt, **aber falsch** → dritte Prüfung „passen die Besetzungen zueinander"
- **Das Rollen-Skript hat die betroffene Rolle von selbst gefunden**, ohne dass es jemand gesagt hat

**Wer die 19 gefunden hat und mit welcher Methode — OFFEN.**

---

## ⚠️ DAS FORMATPROBLEM — Ursache geklärt

### Es liegt NICHT an der Formatumwandlung

Im Selektionsbild des Reports gibt es eine Option für einen **tabulatorgetrennten Export für Tabellenkalkulationen**. Diese Option kam erst mit einer Änderung im **April 2024** in den Report (*„correction list layout and new ALV layout"*).

> ### Der Export ist von 2019 — also aus der Zeit VOR dieser Korrektur. Der alte Listendruck ist die Ursache. Ein neuer Export liefert saubere Spalten.

### Die Symptome
| Symptom | Detail |
|---|---|
| **Kopfzeile steht 1.821× als Datenzeile** | gedruckte Liste mit Seitenumbrüchen, Spaltennamen pro „Seite" wiederholt |
| **Umlaute zerschossen (Mojibake)** | UTF-8-Bytes als GBK interpretiert. Reparatur in Python: `text.encode('gbk').decode('utf-8')` mit Fallback |
| **Kopfzeilennamen um ein Zeichen verschoben** | Namen laufen in die Nachbarzelle über |
| **Mehrere SAP-Felder in einer Excel-Zelle** | rechts im Blatt; erkennbar an Werten wie `"X    1"` oder zwei Codes mit Leerzeichen getrennt |

### Screenshot-Erkenntnisse aus dem Selektionsbild (erklären die Daten)
| Einstellung | Folge im Datensatz |
|---|---|
| Hilfs- und Textpositionen **ausblenden** war aktiv | **deshalb nur die Positionstypen `L` und `X`** |
| Alternativpositionen ausblenden war **nicht** aktiv | **Alternativpositionen sind drin** → Ursache der ~5.300 Nullmengen |
| „Auflösen nach Dummystruktur" war aktiv | Dummys werden durchgerechnet |
| Informationspositionen verbergen war **nicht** aktiv | die ~9.000 `X`-Positionen sind drin |
| Selektion über **Fertigungsversion** | nicht über Stücklistenverwendung |
| Einsatzmenge = 1 | Mengen beziehen sich auf ein Stück des Kopfmaterials |

---

## DIE AUTORITATIVE SPALTENLISTE — 36 Felder

Quelle: die **Ausgabestruktur des Reports** im SAP-Dictionary plus die Datenelement-Bezeichnungen.
**Das ist die Referenz — nicht eine Rekonstruktion aus dem zerschossenen Export.**

| # | Datenelement | Offizielle Bezeichnung | im Export? |
|---|---|---|---|
| 1 | `MATNR` | Materialnummer *(Erzeugnis)* | ✅ |
| 2 | `MAKTX` | Materialkurztext *(Erzeugnis)* | ✅ |
| 3 | `IDNRK` | Stücklistenkomponente | ✅ |
| 4 | `OJTXP` | Objektkurztext (Position) *(Komponente)* | ✅ |
| 5 | `VERID` | **Fertigungsversion** | ❌ **fehlt** |
| 6 | `DGLVL` | Auflösungsstufe (aufbereitet) | ✅ |
| 7 | `CS_E_MNGKO` | errechnete Komponentenmenge in Komponenten-ME | ⚠️ eine der drei |
| 8 | `MEINS` | Basismengeneinheit | ✅ |
| 9 | `POSTP` | Positionstyp (Stückliste) | ✅ |
| 10 | `DTVON` | Datum gültig ab/von | ✅ |
| 11 | `SPOSN` | Nummer der Stücklistenposition | ✅ |
| 12 | `SCHGT` | Kennzeichen: Schüttgut *(Position)* | ⚠️ |
| 13 | `CS_DISPO` | **Auflösungssteuerung** | ⚠️ verschmolzen |
| 14 | `CS_SOBSL` | **Sonderbeschaffungsart der Stücklistenposition** | ⚠️ verschmolzen |
| 15 | `CS_ALPGR` | Alternativposition: Gruppierung | ✅ |
| 16 | `CS_ALPRF` | Alternativposition: Rangfolge | ✅ |
| 17 | `CS_ALPST` | Alternativposition: Strategie | ✅ |
| 18 | `EWAHR` | Alternativposition: **Einsatzwahrscheinlichkeit in %** | ✅ |
| 19 | `SOBSL` | **Sonderbeschaffungsart** *(Materialstamm)* | ✅ |
| 20 | `SCHGT` | Kennzeichen: Schüttgut *(Materialstamm)* | ✅ |
| 21 | `RGEKM` | **Kennzeichen: retrograde Entnahme** | ✅ |
| 22 | `SANKO` | Konstruktionsrelevanz Position | ✅ |
| 23 | `SANFE` | Fertigungsrelevanz Position | ✅ |
| 24 | `SANKA` | Kalkulationsrelevanz Position | ⚠️ mit 25 verschmolzen |
| 25 | kundeneigenes Feld | Alternativkomponente | ⚠️ mit 24 verschmolzen |
| 26 | `ATWRT` | Merkmalwert *(Lizenznehmer)* | ✅ |
| 27 | `ATWRT` | Merkmalwert *(**Bauteilart**)* | ✅ |
| 28 | `ATWRT` | Merkmalwert *(Auftragsdruck)* | ✅ |
| 29 | `HRKFT` | Herkunftsgruppe | ⚠️ mit 30 verschmolzen |
| 30 | `BKLAS` | Bewertungsklasse | ⚠️ mit 29 verschmolzen |
| 31 | `BESKZ` | Beschaffungsart *(Eigen/Fremd/beides)* | ✅ |
| 32 | `VERKZ` | Verarb.kennzeichen Sonderbestandspflege | ⚠️ mit 33 verschmolzen |
| 33 | `ATWRT` | Merkmalwert *(Klassifizierung)* | ⚠️ mit 32 verschmolzen |
| 34 | `STLST` | Stücklistenstatus | ✅ |
| 35 | `CS_E_MNGKO` | **Komponentenmenge gewichtet mit Einsatzwahrscheinlichkeit** | ❌ **fehlt** |
| 36 | `KMPMG` | Komponentenmenge | ⚠️ eine der drei |

### Drei Konsequenzen

**1. Die linke Hälfte (Felder 1–11) ist eindeutig zuordenbar.** Damit kann sofort gearbeitet werden.

**2. Die rechte Hälfte nicht.** Dort sind Felder verschmolzen — und **genau dort liegen die beiden Sonderbeschaffungs-Felder** (14 und 19), also die Grundlage der Dummy-Regel R5/R6. Im Export gibt es **zwei Kandidatenspalten** mit plausiblen Werten und unterschiedlichem Füllgrad (~33 % und ~38 %).
> **Wer auf der falschen filtert, produziert Müll, der niemandem auffällt.**

**3. Dem Export fehlen Spalten, die es heute gibt.** Fertigungsversion (seit ~2021) und die EWAHR-gewichtete Menge (seit 2024). Deshalb war die gewichtete Menge nur **in der Transaktion** sichtbar, nicht in der Datei.

> ### → Ein neuer Export gibt 36 saubere Spalten statt 27 verschmolzener. Belegt, nicht vermutet.

### Ungelöster Widerspruch zur Mengenspalte
In der SAP-Tabelle steht bei einer Alternative mit Einsatzwahrscheinlichkeit 0 die **rohe** Menge ungleich Null — nur die **gewichtete** ist 0. Der Export hat aber nur **eine** Mengenspalte, und die zeigt ~5.300 Nullen.

**Vermutung (nicht belegt):** Der alte Listendruck zeigte bereits den **gewichteten** Wert. Passt zum Änderungsprotokoll von 2024, das genau die Anzeige der Menge bei Einsatzwahrscheinlichkeit 0 korrigiert und dafür eine eigene Spalte nachgezogen hat.

→ Hängt an der offenen Frage, **welche der drei Mengenspalten** im Export steht.

### Drei Selbsttests zur Spaltenidentität (noch nicht durchgeführt)
Falls doch auf dem alten Stand gearbeitet wird — prüfen statt glauben, ohne Fachseite und ohne neuen Export:
1. **Zeilen mit Dummy-Kennzeichen müssen Elternteile sein** (ein Phantom ist eine Baugruppe mit Kindern). Sind sie Blätter → falsche Spalte.
2. **Die Einsatzwahrscheinlichkeit muss sich innerhalb einer Alternativgruppe auf 100 summieren** (am System bestätigt).
3. **Menge 0 muss überwiegend mit Einsatzwahrscheinlichkeit 0 zusammenfallen.**
