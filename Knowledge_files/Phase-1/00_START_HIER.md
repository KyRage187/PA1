# 00 — START HIER

**Dieses Verzeichnis ist der Wissensstand des Projekts.**
Ein neuer Chat sieht den vorherigen Gesprächsverlauf **nicht**. Alles Nötige steht hier.

Lesereihenfolge: `00` → `01` → `04` → `03` → `02` → `05` → `FRAGEN.md`

| Datei | Inhalt |
|---|---|
| `00_START_HIER.md` | Status, nächster Schritt, Arbeitsregeln *(volatil)* |
| `01_PROJEKT.md` | Rahmen, Ziel, Scope, Historie der Ansätze und **warum** |
| `02_SAP_WISSEN.md` | SAP-/Stücklisten-Fachwissen — das Wertvollste |
| `03_DATENSATZ.md` | Struktur des Exports, Spaltenreferenz, bekannte Mängel |
| `04_ENTSCHEIDUNGEN_REGELN.md` | Entscheidungen D1–D8, Cleaning-Regeln R1–R12 |
| `05_CODE.md` | Was existiert, Ergebnisse, bekannte Bugs |
| `FRAGEN.md` | Offene Fragen, getrennt nach AI und Mensch |

---

## ⚠️ VERTRAULICHKEIT — bitte einhalten

Diese Dokumentation ist **absichtlich unternehmensneutral** gehalten.

**Nicht in diese Dateien aufnehmen:**
- Materialnummern, Stücklistennummern, Auftragsnummern
- Produkt-, Typ- oder Baureihenbezeichnungen
- Materialkurztexte im Wortlaut
- Kunden-, Lieferanten- oder Personennamen
- Werksnummern, Standorte, Unternehmensname
- Namen kundeneigener SAP-Objekte (Reports, Strukturen, Pakete, Varianten), Systemkennungen, Benutzerkennungen

**Warum das kein Verlust ist:** Konkrete Daten sind jederzeit aus dem Export und aus SAP wieder herleitbar. Entscheidungen, Korrekturen und gelernte Lektionen sind es **nicht**. Diese Dateien tragen genau das, was nicht rekonstruierbar ist.

**Platzhalter-Konvention:** `<UNTERNEHMEN>`, `<STANDORT>`, `<WERK>`, `<Z-REPORT>` (der kundeneigene Stücklisten-Auflösungsreport), `<ALV-STRUKTUR>` (dessen Ausgabestruktur), `<VARIANTE>`, `<BAUREIHE-A/B>`, `<MATNR>`, `<BB>` (betrieblicher Betreuer), `<AP>` (fachlicher Ansprechpartner).

---

## STATUS (Ende Tag 1)

**Wir sind mitten im Data Cleaning, Schritt 1 abgeschlossen.**

Erledigt:
- Export vollständig profiliert (alle Spalten, alle Blätter grob)
- Drei Analyseansätze gebaut und bewertet → siehe `05_CODE.md`
- **Autoritative Spaltenliste aus der Transaktion geholt: 36 Felder** → siehe `03_DATENSATZ.md`
- Damit belegt: der vorliegende Export hat nur 27 Spalten, rechts mehrere verschmolzen, zwei fehlen ganz

**Offene Entscheidung, direkt als nächstes:**
> Mit dem alten Stand die **linke Hälfte** (Felder 1–11, eindeutig zuordenbar) aufräumen —
> **oder** erst einen **neuen, sauberen Export** besorgen?

**Warum das die Blockade ist:** Die Dummy-Regel (R5/R6) betrifft die Felder für Sonderbeschaffung — beide liegen in der zerschossenen rechten Hälfte. Auf dem alten Stand könnte die falsche Spalte gefiltert werden, ohne dass es auffällt.

---

## ARBEITSREGELN — bitte einhalten

Ausdrückliche Anweisungen des Bearbeiters, mehrfach eingefordert:

1. **Beide geben Go, bevor etwas gemacht wird.** Vorschlagen → warten → machen. Nicht vorgreifen, nicht „schon mal bauen". Wurde mehrfach eingefordert, weil ich zu oft einen Schritt weiter war.
2. **Kein Gelaber.** Direkt, knapp, keine Vorreden.
3. **Immer die echten Spaltennamen des Exports verwenden**, nicht abstrakte Eigenbegriffe. Auf ausdrücklichen Wunsch.
4. **Code immer in dieses Projektverzeichnis.** Dateien außerhalb sind im Workspace des Bearbeiters **unsichtbar** — ist schon einmal passiert.
5. **Klar trennen: Befund / Vermutung / Entscheidung.** Nichts als gesichert darstellen, was rekonstruiert ist.
6. **Partner-Modus.** Beide bringen Ideen, beide dürfen widersprechen.
7. **Struktur für alles Neue:** `Fragen AI` / `Fragen Mensch` / `ToDo` / `Phasen`.
8. **Das Projekt ist nicht an einem Tag zu lösen.** Alles braucht einen Plan. Tempo drosseln, nicht Ergebnisse forcieren.

---

## SYSTEME UND WERKZEUGE

| | Rolle | Bedeutung fürs Projekt |
|---|---|---|
| **Operatives System** | Produktivsystem | **Der vorliegende Export stammt hierher.** Kein Zugriff für die KI. |
| **Testsystem** | Folgesystem | KI hat **Lesezugriff** — für SAP-Semantik, Domänenwerte, Prüftabellen, Report-Quellcode. Daten können vom Produktiven abweichen. |
| **Entwicklungssystem** | Quelle der Z-Objekte | kein Zugriff |
| Ein weiteres Testsystem (BW) | – | bisher nicht genutzt |

> ⚠️ **Wichtige Konsequenz:** Bei Widersprüchen zwischen Export und Testsystem gilt **der Export** — er kommt aus dem operativen System.

**Die angebundenen Systeme und ihre IDs stehen im System-Prompt.** Nicht hier dokumentieren.

| Werkzeug | Wofür | Grenze |
|---|---|---|
| Lesezugriff Testsystem | SAP-Semantik klären. **Damit sind für Feldbedeutungen keine Mittelsmänner nötig.** | Testdaten |
| Python im Container | `openpyxl`, `pandas`, `pyxlsb` installiert (PEP-668: `--break-system-packages`) | – |
| **Warenbewegungen** (Materialbelege, Reservierungen, Auftragsbewegungen) | **kein direkter Zugriff.** Nur über Mittelsmänner, unzuverlässig. Möglich: einzelne Abfragen + einmal erklären lassen. | sparsam — für die Validierungsstichprobe reservieren |
| ML-Plattform (Databricks o. ä.) | soll geprüft werden | offenes ToDo |

---

## HINWEISE ZUM CODESTAND

- Eine frühere Anleitung zur lokalen Ausführung ist aus dem Ordner verschwunden — Inhalt gerettet in `05_CODE.md`.
- Drei Wegwerf-Analyseskripte fehlen ebenfalls, kein Verlust.
- Der Bearbeiter hat lokal auf relative Pfade via `pathlib` umgestellt (D7). **Seine lokale Version ist aktueller als die im Container.**
