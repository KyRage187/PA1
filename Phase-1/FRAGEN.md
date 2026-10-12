# FRAGEN

> Struktur nach Vorgabe des Bearbeiters: **FRAGEN AI** (selbst klärbar) / **FRAGEN MENSCH** (braucht Fachseite) / **TO DO**.
> Beantwortete Fragen bleiben **mit Antwort** stehen — sonst geht das Wissen verloren.
> Unternehmensneutral formuliert.

---

# FRAGEN AI

## Offen

| # | Frage | Anmerkung |
|---|---|---|
| A1 | **Kommt Menge = 0 nur bei Stückgut vor oder bei allen Mengeneinheiten?** | Ausdrücklich vom Bearbeiter gefragt. In einer Minute am Export prüfbar. **Go fehlt noch.** |
| A2 | Domänentexte zur Sonderbeschaffung am System nachziehen | Abfrage lief zweimal in einen HTTP-403; tritt sporadisch auf, Wiederholung hilft |
| A3 | Was steht in den **drei nie ausgewerteten Blättern** der Arbeitsmappe (Selektionsblatt, Erzeugnisliste, Änderungsblatt 2017–2019)? | könnte relevant oder Altlast sein |
| A4 | Die **drei Selbsttests zur Spaltenidentität** durchführen | siehe `03_DATENSATZ.md`, letzter Abschnitt |
| A5 | **Nummernkreise neu und einfacher erklären** | Meine Erklärung ist **nicht angekommen**. Mein Fehler, nicht der des Bearbeiters. |
| A6 | Was bedeutet „buchbar"? | Mein Begriff, wurde als unklar markiert. Muss definiert werden. |
| A7 | Was war mit „Phase 3" gemeint? | Gemeint war: den Buchungsweg einmal von einem Menschen erklären lassen. Muss klarer formuliert werden. |
| A8 | Rechte Spalten am System gegen echte Daten verifizieren | macht aus der Rekonstruktion belegtes Wissen |

## Beantwortet

| Frage | Antwort |
|---|---|
| Kommt das Formatproblem von der Formatumwandlung? | **Nein.** Der Report hatte 2019 nur den alten Listendruck. Die Option für tabulatorgetrennten Export kam erst 2024. **Ein neuer Export löst es.** |
| Warum enthält der Export nur zwei Positionstypen? | Hilfs- und Textpositionen wurden beim Export **ausgeblendet** (Häkchen im Selektionsbild). |
| Woher kommen die ~5.300 Nullmengen? | **Alternativpositionen mit Einsatzwahrscheinlichkeit 0.** Das Ausblenden von Alternativpositionen war **nicht** aktiv. **Kein Fehler.** |
| Warum haben zwei Alternativen unterschiedliche gewichtete Mengen? | Sie liegen in derselben Alternativgruppe. Die Einsatzwahrscheinlichkeit verteilt 100 % innerhalb der Gruppe, die gewichtete Menge ist `Menge × EWAHR/100`. **Und es ist eine Eigenschaft der Position, nicht des Materials** — dasselbe Teil hat in anderen Stücklisten den umgekehrten Wert. |
| Wie heißen die Spalten wirklich? | **36 Felder**, Quelle ist die Ausgabestruktur des Reports im Dictionary. Vollständige Liste in `03_DATENSATZ.md`. |
| Eine oder zwei Sonderbeschaffungs-Spalten? | **Zwei** — eine aus der Stücklistenposition, eine aus dem Materialstamm. **Schüttgut ebenfalls zweimal.** |
| Wie heißt die Selektionsvariante? | Der Bearbeiter hatte einen Buchstaben falsch notiert; der korrekte Name steht im System. |
| Was bedeutet „Mont" in den Notizen? | **Montage.** Steht als Kommentar im Quellcode des Reports. |
| Nutzt der Standort Variantenkonfiguration? | **Nein** — nur eine Handvoll konfigurierbarer Materialien gegen Millionen diskreter. Es liegt **Modell B** vor (siehe `02_SAP_WISSEN.md`). |
| Ist retrograde Entnahme relevant? | **Ja, bei rund der Hälfte** aller Material/Werk-Sätze aktiv. Regelfall. |
| Welches System liefert den Export? | **Das operative System** (D8). Bei Widersprüchen gilt der Export, nicht das Testsystem. |

---

# FRAGEN MENSCH

## Kritisch — blockieren die Arbeit

| # | Frage | Warum kritisch |
|---|---|---|
| **M1** | **Was ist eine Baugruppe in der Stückliste?** | ⚠️ Der Bearbeiter hat das erklärt und wurde **schief angeschaut**. Meine Behauptung „eindeutig ableitbar" ist **zurückgezogen**. Ohne Antwort steht die ganze Baumlogik auf wackligem Grund. |
| **M2** | **Zielsatz schriftlich bestätigen lassen** | *„Warum werden andere Sachen gebucht als in der Tabelle stehen?"* — ein Satz vom Betreuer oder Ansprechpartner. Der letzte Vortrag lief flach, weil das nicht festgenagelt war. |
| **M3** | **Welche der drei Mengenspalten steht im Export?** | Betrifft D1 direkt und erklärt den ungelösten Widerspruch zu den Nullmengen. |
| **M4** | **Bezieht sich die Menge auf das direkte Überteil oder auf das Fertiggerät?** | D1 setzt „wie sie dasteht" als Arbeitsentscheidung; fachlich ist es offen. Blockiert Mengenauswertung über mehrere Stufen. |
| **M5** | **Kann ein neuer, sauberer Export erzeugt werden?** | ALV-Export statt Listendruck → **36 saubere Spalten statt 27 verschmolzener.** Kleinster Aufwand, größte Wirkung. |

## Fachlich — Alternativpositionen

| # | Frage |
|---|---|
| M6 | Was bedeutet die **Alternativposition-Strategie**, Werte `1` und `2`? |
| M7 | Warum gibt es **mehrere Alternativgruppen** in einer Stückliste? |
| M8 | Warum steht bei manchen Alternativen die **erste Wahl auf 0 %** und die zweite auf 100 %? Wird die erste Wahl nicht mehr benutzt? |
| M9 | Sollen Alternativpositionen im Export überhaupt mitkommen, oder besser an der Quelle ausblenden? (R11) |

## Fachlich — Dummys und Cleaning

| # | Frage |
|---|---|
| M10 | Sonderbeschaffungs-Werte bestätigen: **was ist der zweite Dummy-Wert genau?** |
| M11 | Bei „Dummy raus" — **beide** Sonderbeschaffungs-Spalten oder eine bestimmte? Der Bearbeiter sagte „egal welche"; zur Sicherheit bestätigen. |
| M12 | Sollen **Informationspositionen** raus? (R8 — ich habe sie bisher gefiltert, **ohne Entscheidung**) |
| M13 | Soll **Schüttgut** raus? (R9) |
| M14 | **Stichtag** für das Gültig-ab-Datum setzen? Stände von 2001 neben 2019. (R10) |

## Prozess und Buchungen

| # | Frage |
|---|---|
| M15 | **Wie kommt eine Buchung zustande?** Retrograde Entnahme, Rückmeldung, Nachbuchung — einmal komplett erklären lassen. |
| M16 | **Wer korrigiert, wann und warum?** |
| M17 | **Wie oft und wie stark weichen Buchungen ab?** Größenordnung genügt. Der Bearbeiter: *„keine Ahnung, aber es passiert, und das ist das Entscheidende."* |
| M18 | **Was kostet ein Fehler?** Materialwert, Inventurdifferenz, Nacharbeit. Braucht die Arbeit für die Einleitung — und rechtfertigt das Projekt. |
| M19 | **Wie validieren wir den Soll- und Ist-Prozess?** (eigene Formulierung des Bearbeiters) |
| M20 | Gibt es bereits Auswertungen zu **Inventurdifferenzen** am Standort? |

## Fehlerbild und Validierung

| # | Frage |
|---|---|
| M21 | **Gibt es weitere bestätigte Fehlerfälle** außer den 19 geprüften? Die sind alle vom selben Typ. |
| M22 | **Wer hat die 19 gefunden und mit welcher Methode?** Waren es echte Fehler oder Fehlalarme? |
| M23 | Welche Fehlerarten kommen real vor? Menge falsch, Teil zu viel, Teil fehlt, falsche Variante, falscher Positionstyp, veralteter Gültigkeitsstand? |
| M24 | **10 Rollen aus der Ausgabe** der Montage zeigen: „Sind das austauschbare Ausführungen desselben Bauteils?" → Trefferquote |

## Struktur und Systematik

| # | Frage |
|---|---|
| M25 | **Gibt es in SAP schon ein Feld für Baureihe / Produktgruppe?** Produkthierarchie (`PRDHA`), Warengruppe (`MATKL`), Klassifizierung, ein kundeneigenes Feld? **Wenn ja, wäre das Gruppierungsproblem gelöst** und wir müssten keine Textanalyse rechtfertigen. |
| M26 | **Ist die Materialnummer systematisch aufgebaut? Gibt es eine Dokumentation der Systematik?** Bestätigen: codiert ein Buchstabe die Objektart, ist ein Zahlensuffix für „Basisteile" reserviert, codieren mittlere Ziffernblöcke Teileklassen? |
| M27 | Gilt „Nummernkreise nicht analog aufgebaut" nur für **Bauteile** oder auch für **Erzeugnisse**? Bei Erzeugnissen wirkt der Stamm systematisch — mit einem gefundenen Gegenbeispiel. |
| M28 | Ist der **Materialkurztext nach Regeln gepflegt**? Es wurde ein Kurztext mit fehlendem Anfangsbuchstaben gefunden. Wie häufig sind solche Tippfehler? |
| M29 | Was ist das **Klassifizierungsmerkmal zur Bauteilart**, wer pflegt es, **warum nur 19 % gefüllt?** Wäre der billigste Weg zur Substitutionserkennung. |
| M30 | Dürfen Geräte derselben Baureihe **legitim stark unterschiedliche** Stücklisten haben? Gibt es ein Kennzeichen, das kundenspezifische Varianten markiert? |

## Organisatorisch

| # | Frage |
|---|---|
| M31 | Wer schaut die Kandidatenliste an, wie viele Einträge pro Woche sind zumutbar? |
| M32 | Was ist schlimmer — einen Fehler übersehen oder Fehlalarm? **Bestimmt die Schwellwerte.** |
| M33 | Soll das Ergebnis später in SAP laufen oder als separates Tool? (nur für den Ausblick) |
| M34 | Kann das **operative System** als System in der Arbeitsumgebung angebunden werden? Der Bearbeiter hat Zugriff, die KI nicht. |
| M35 | Zitierstil mit dem wissenschaftlichen Betreuer klären; ist ein **Sperrvermerk** nötig? |

---

# TO DO

| # | Aufgabe | Wer |
|---|---|---|
| T1 | Sauberen Export ziehen bzw. Formatierung in Ordnung bringen | Bearbeiter |
| T2 | ~~Prüfen, ob die Formatumwandlung schuld war~~ **erledigt: nein, es war der Export selbst** | – |
| T3 | Die Felder durchgehen, die entscheiden **ob** eine Position gebucht wird — **in Ruhe** | beide |
| T4 | Phase 3 und Phase 4 inhaltlich klären | beide |
| T5 | **Phasen neu und feiner nummerieren** — *„Phase 5 ist in der Realität eher Phase 10"* | beide |
| T6 | **ML-Plattform (Databricks o. ä.) prüfen** statt nur lokales Python | beide |
| T7 | **Bereinigungsskript bauen**, sobald die Export-Frage entschieden ist | KI, nach Go |
| T8 | Rollenerkennung **je Baureihe** statt global | KI, nach Go |
| T9 | Bug beheben: Filter vor Baumbau kostet ~5 % der Zeilen | KI, nach Go |
| T10 | Skript 2 mit „alle Stufen" testen — eine Zeile, nie ausprobiert | KI, nach Go |
| T11 | **Diese Dokumentation unternehmensneutral halten** — bei jedem Update prüfen | KI, laufend |
