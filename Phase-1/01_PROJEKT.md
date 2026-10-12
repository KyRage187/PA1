# 01 — PROJEKT

> Unternehmensneutral gehalten. Platzhalter-Konvention siehe `00_START_HIER.md`.

## Rahmen

| | |
|---|---|
| **Bearbeiter** | Studierender, Data Science und Künstliche Intelligenz, Duale Hochschule, 2. Praxissemester |
| **Praxispartner** | `<UNTERNEHMEN>`, Produktionsstandort `<STANDORT>` (`<WERK>`) |
| **Betrieblicher Betreuer** | `<BB>` |
| **Fachlicher Ansprechpartner** | `<AP>` — betont immer wieder „Pattern Matching", war **skeptisch, ob KI mit SAP sowas überhaupt hinbekommt** |
| **Wissenschaftlicher Betreuer** | Studiengangsleiter der Hochschule |
| **Arbeitszeit** | ca. 7:30–15:45, Fokus vormittags |

## Formales

- **Typ:** Projektarbeit 1 (PA1)
- **Thema:** Entwicklung eines Prototyps zur Erkennung von Dateninkonsistenzen in SAP-Stücklisten mittels Machine Learning, am Beispiel eines Produktionsstandorts
- **Umfang:** 20–30 Seiten reiner Inhalt (ohne Abbildungen, Tabellen, Verzeichnisse, Anhänge)
- **Abgabe:** digital + gebundenes Papierexemplar + Abstract + Ablauf-/Reflexionsbericht
- **Kein Kolloquium**, keine mündliche Verteidigung
- **Template:** LaTeX-Vorlage des Studiengangs
- **Zitierstil:** noch mit dem wissenschaftlichen Betreuer abzustimmen
- **Sperrvermerk:** ggf. nötig, wird im Verlauf geklärt
- **Sprache:** Deutsch
- **Arbeitsweise:** Projekt und Dokumentation laufen **parallel**

## Vorläufige Gliederung

1. Einleitung
2. Grundlagen zu SAP-Stammdaten, Stücklisten und Variantenfertigung
3. Projektkontext und Datenbasis
4. Methodisches Vorgehen zur Muster- und Anomalieerkennung
5. Umsetzung des Ansatzes
6. Ergebnisse und Bewertung
7. Fazit und Ausblick

---

## DAS ZIEL

Nach einem Vortrag, der **nicht gut lief**, wurde das Ziel erstmals klar formuliert:

> ### „Warum werden andere Sachen gebucht, als in der Tabelle stehen?"

Das ist **etwas anderes** als „unplausible Stücklisten finden". Der Vortrag lief flach, weil Stücklistenstruktur gezeigt wurde, während die Stakeholder Buchungsabweichungen erwarten. **Diese Zielverschiebung ist die wichtigste Erkenntnis des Tages.**

### Die zweite, ebenso wichtige Vorgabe

> ### Der Algorithmus ist das Produkt, nicht die Auswertung eines einzelnen Exports.

Wörtlich: Datensätze können komplett neu sein, *„der Algorithmus muss sitzen"*, und er *„soll mit ALLEM funktionieren"*. Daraus folgt zwingend:

- **keine fest verdrahteten Baureihen**
- **keine fest verdrahteten Nummernkreise**
- **keine Annahmen über Produktbereiche** — andere Produktfamilien müssen genauso durchlaufen
- Spalten müssen über **Namen** erkannt werden, nicht über Position
- Der Code muss mit einem Datensatz klarkommen, den niemand vorher gesehen hat

**Das ist der Grund, warum „lernen statt parsen" die richtige Richtung ist.** Ein Parser wäre das Gegenteil von allem-funktionieren.

### Ausgangslage fachlich

- Der Standort betreibt **Variantenfertigung**: Grundgeräte mit vielen Varianten
- Jede Variante hat in SAP eine **eigene** Stückliste
- SAP prüft Stücklisten **syntaktisch und referenziell, aber nicht semantisch** — ein doppelt eingetragenes Bauteil wird akzeptiert
- Durch **retrograde Entnahme** wird die Stückliste zur **Buchungsvorschrift**: Fehler werden automatisch und unbemerkt gebucht
- Folgen: Materialüberverbrauch, Inventurdifferenzen, SAP ≠ Realität
- Keine systematische automatisierte Erkennung vorhanden

### Was der Prototyp leisten soll

Formulierung des Bearbeiters:

> „Mein ML soll nur **potentielle** Fehler / Ausreißer anzeigen, damit jemand mit Domänenwissen über eine Liste mit vielleicht 50 Einträgen schaut und sieht: ok, was **könnte** aufgrund statistischer Wahrscheinlichkeit ein Ausreißer sein."

Also **Triage-Werkzeug, kein Urteil.** Kein „das ist falsch, weil …", sondern „das weicht ab, schau hin" — mit Evidenz, damit ein Mensch in Sekunden entscheiden kann.

---

## HISTORIE DER ANSÄTZE — und warum das in die Arbeit gehört

Drei Ansätze, zwei gescheitert. **Das Scheitern ist das Ergebnis**, nicht die Panne. Der Bogen ist eine echte Herleitung mit Wendung.

### Ansatz 1 — direkt nach Anomalien suchen
Statistik pro Produktfamilie, Abweichungen flaggen.
→ **85 % aller Geräte geflaggt.** Unbrauchbar.
→ **Erkenntnis:** Kundenspezifische Varianten haben **planmäßig** Sonderteile. Das Problem ist nicht, Abweichungen zu finden — es ist, die **legitimen auszusortieren**. Präzision, nicht Trefferzahl.
→ Nach Abschalten des Signals „seltenes Teil": 18 statt 1.246 Kandidaten.

### Ansatz 2 — erst Geräte gruppieren, drei unabhängige Wege
Über Kurztext / über Materialnummernstamm / über Teilestruktur.
→ **Kurztext und Nummernstamm stimmen zu 97,7 % überein** — zwei unabhängige Signale, dieselbe Hierarchie, zweistufig.
→ Der strukturbasierte Weg **scheiterte** (Fingerabdruck zu dünn, Ursache verstanden — siehe `05_CODE.md`).

### Ansatz 3 — Erkenntnis und Umkehrung
Alle drei Verfahren **lesen Bedeutung aus Bezeichnern**. Das setzt voraus, dass man die Systematik kennt — also genau das Domänenwissen, das erst erzeugt werden soll.

> **Ein Parser braucht das Wissen als Eingabe. Wir wollen es als Ergebnis.**

Daraus der Rollen-Begriff (siehe `02_SAP_WISSEN.md`), gelernt aus dem Verbauzusammenhang statt aus Namen. **Das funktioniert** — Belege in `05_CODE.md`.

### Die Ergänzung des Bearbeiters, die das Modell vervollständigt

Er hat selbst erkannt, dass Rollen allein nicht reichen:

> „Es muss auch geprüft werden, **ob die Rolle richtig besetzt** wurde — sonst könnten ganz neue Gerätschaften entstehen."

Also drei Prüfungen statt einer:
```
1.  IST die Rolle besetzt?             → fehlendes Teil
2.  WIE OFT ist sie besetzt?           → Bauteil doppelt
3.  PASSEN die Besetzungen zueinander? → seine Ergänzung, noch offen
```
Beispiel für 3: eine Baugruppe für eine Spannungsvariante und ein Typenschild für eine **andere** Spannung besetzen **beide korrekt** ihre Rolle — die Kombination ist trotzdem falsch. **Genau das ist das Muster der geprüften Abweichungsfälle** (siehe `03_DATENSATZ.md`).

---

## PHASENPLAN (Stand, ausdrücklich unfertig)

| Phase | Inhalt | Status |
|---|---|---|
| **0** | Sauberer Export (ALV-Export statt Listendruck) | ✅ einig, offen |
| **1** | Datenbereinigung | ✅ einig, in Arbeit |
| **2** | Gruppierung absichern — „Töpfe nicht vermischen", Baureihen nicht mischen | ✅ einig |
| **3** | *(unklar)* — gemeint war: Buchungsweg einmal erklären lassen | ⬜ klären |
| **4** | *(unklar)* — gemeint war: Risikoprofil aus den rechten Spalten ableiten | ⬜ ToDo |
| **…** | **es fehlen Schritte** — nämlich der Algorithmus selbst | ⬜ |
| **5 → eher 10** | Betreuer prüft die Funde, urteilt ja/nein | ✅ einig |

**Kritik des Bearbeiters, wörtlich:** *„da fehlt ganz klar ein Schritt zu Phase 5, Phase 5 ist ja schon das Ende, das ist wohl in der Realität eher Phase 10."* → **Nummerierung muss feiner geschnitten werden.**

### Realistischer PA1-Schnitt (Vorschlag, nicht entschieden)
- **Drin:** Phase 0, 1, 2 vollständig + Algorithmus + Machbarkeitsnachweis an einer Stichprobe
- **Ausblick:** vollautomatischer Soll/Ist-Vergleich, weitere Produktbereiche, produktives Tool

### Angepasste Forschungsfrage (Vorschlag, nicht bestätigt)
> Wie lassen sich aus SAP-Stücklisten die funktionalen Rollen von Bauteilen und ihre Zugehörigkeit ableiten, **ohne die Nummern- und Benennungssystematik als Vorwissen vorauszusetzen** — und wie lassen sich daraus Positionen mit hohem Risiko für Buchungsabweichungen priorisieren?

---

## WEN EINBEZIEHEN

| Wer | Wofür |
|---|---|
| **Montage** | Was wird wirklich verbaut? Urteil über Rollen. Der Report wird laut Quellcode-Kommentar *„in der gesamten Montage genutzt"* |
| **Stammdatenpflege** | Wer pflegt Stücklisten, nach welchen Regeln, wo entstehen Fehler |
| **Fertigungssteuerung** | Backflush-Einstellungen, Fertigungsversionen, warum korrigiert wird |
| **Controlling / Inventur** | **Wichtigster Kontakt** — dort liegt die Zahl, die das Projekt rechtfertigt |
| **SAP-seitige Mittelsmänner** | Buchungsstichprobe, sparsam einsetzen |
| **Betreuer / Ansprechpartner** | Zielbestätigung, Scope, Urteil über Funde |
