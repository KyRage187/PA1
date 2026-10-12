\# FRAGEN — Big3/Big2-Stand und historisches Register



\## Aktuell offen, 03.10.2026 (Cmd 8-Fachbereichs-Erklärung)



\### Abby/G, Code und Daten



1\. \*\*Cmd 8 v4 wurde noch nicht an echten Daten getestet.\*\* Nur Syntax-/Logikprüfung mit synthetischen Testdaten in der Sandbox. Nutzer muss in Databricks laufen lassen und Ergebnis zurückmelden.

2\. `HAUPTGRUPPEN = ("GJL121", "GJL131")` in Cmd 2 muss generalisiert werden, bevor Tests mit neuen Stücklisten/Hauptgruppen (TF-Stückliste genannt) sinnvoll sind.

3\. Vorschlag „Vergleichsprodukte nach Typbezeichnung gewichten“ (Punkt 1 des 5-Punkte-Plans) noch nicht freigegeben — braucht gesondertes Go, da Eingriff in Cmd 5/6.

4\. Cmd 9 (separate Bündelzelle) ist obsolet, sollte aus dem Notebook gelöscht werden — Logik ist jetzt in Cmd 8 v4 integriert.

5\. Für die Spaltenstruktur von eventuellen weiteren Stücklisten (TF, ggf. mehr) werden Spaltennamen/Beispielzeilen benötigt, falls sich das Schema vom aktuellen `sauber`-Format unterscheidet.



\### Fachbereich (aus der Fallanalyse der Testtabelle, konkrete offene Fragen)



\- \*\*`VB7D-30-01\_24VDC\_VE5\_AUMA` vs. `VB7D-30-01\_24VDC\_VE20\_AUMA`:\*\* fehlt bei VE5 ein komplettes Kontaktsystem (8 Teile), obwohl VE5/VE20 sich laut Produktlogik nur in der Verpackungseinheit unterscheiden sollten — Fehler oder Struktur?

\- \*\*`BC6-30-10-2.4 17-32VDC ITAL. BAHN`:\*\* Gehäuse, Joch, Jochschenkel, Schrauben, Etikett fehlen gegenüber allen 10 Vergleichsprodukten — Fehler oder bahnspezifische Variante?

\- \*\*`VBC6A-30-10-P 220-240VDC` / `VBC7A-40-00 24VDC`:\*\* alle Mengen durchgehend halbiert — ist das eine bekannte Wendekombinationsstruktur (zweites Schütz als eigene Baugruppe) oder ein Datenfehler?

\- Rückmeldung zur Spule 60V (bestätigter Kontrollfall) bleibt erforderlich für jede neue Modellversion als Gegenprobe.



\### C / Arbeit



\- Dokumentieren: Begriffsklärung Kalibrierung vs. Ranking/Präzision, als eigener kleiner Abschnitt — zeigt methodisches Verständnis in der Praxisarbeit.

\- Versuchsgeschichte Cmd 8 v1→v4 mit den jeweiligen Nutzerkorrekturen (Bloat-Kritik, verlorene Vergleichsprodukte in v3, Rückschritt durch Cmd 9) ist ein gutes Beispiel für iterative Verfeinerung mit Nutzerfeedback — als Negativbeispiel/Lernschritt dokumentierbar, nicht nur Endergebnis zeigen.

\- Fallanalyse-Tabelle (7 Muster, 62 Zeilen) mit Status (bestätigt/Hypothese/offene Frage) ist verwendbar als Tabelle/Abbildung im Kapitel zur Fehlerkategorisierung.



\## Aktuell offen, 01.10.2026 (Big3)



\### G/Code und Daten



1\. \*\*\[Lücke]\*\* Fold-Vergleich (Block 1) des finalen Big3-Laufs 6 liegt nicht vor; nur Lauf 5 (0,029 → 0,016) ist belegt. Vor Zitat beim Bearbeiter nachfragen.

2\. Stacking-Lauf `BASIS\_B3 = "v2+big1"` ausführen und mit der Baseline vergleichen (Code vorhanden, nicht gelaufen).

3\. Big3 auf GJL131 (nur `HAUPTGRUPPE` in Cmd 8 ändern) — nicht gelaufen.

4\. Big3-Modelle und Manifest speichern (TRAIN/INFER-Trennung analog Big2); derzeit nichts gespeichert.

5\. Ausgabespalten für die gemeinsame Liste festlegen: `p\_big3`, `auff\_big3`, zwei stärkste Strukturbegründungen je Zeile.

6\. Gemeinsames Notebook Cleaner → Big1 → Big2 → Big3 → \*\*eine\*\* Liste (GJL121 + GJL131) — vom Bearbeiter als nächster Schritt festgelegt, nicht durch G.

7\. `03\_DATENSATZ.md` um die Cleaner-Strukturfelder und GJL121-Zahlen aus G\_HANDOUT Abschnitt 6 ergänzen (lag G nicht vor).



\### Fachbereich (Weekly 02.10.2026)



\- „Beide hoch“: Spule 60 V (bestätigt), Klebeetikett, Stößel — Fehler?

\- „Big1 hoch, Big3 niedrig“: Joch bei vorhandener, nicht aufgelöster geschweißter Baugruppe; Hilfskontaktbrücke (Bauform); Feder 0,3 N statt 0,4 N — gültige Variante oder Fehler?

\- „Nur Big3 hoch“: 23 Fälle (Stößel, Verpackung, Federn, Festschaltstücke/Kontaktbrücke in zwei Produkten, 6-V-Spule, Etiketten) — Fehler?

\- Ist Gehäuse P0009 gleichwertiger Ersatz für P0008?

\- Antworten als Labels mit Datum und Begründung sichern; keine Ausnahmefilter.



\### Methodisch \[Hypothese/offen]



\- Nachbarmerkmal trägt 36 % der Wichtigkeit: Verlässlichkeit bei Produkten ohne ähnliche Nachbarn?

\- Reduziert Big3 den Prüfaufwand messbar? Erst mit Labels beantwortbar.

\- Varianten/Ersetzungen bleiben unbearbeitet.



\### C / Arbeit



\- Big3-Versuchskette mit Status dokumentieren (G\_HANDOUT Abschnitt 9); Korrektur „Big3 nicht trainiert“ → trainiert. Abgabeziel 16.10.2026.



Details: \[G\_HANDOUT](G\_HANDOUT\_Big3\_2026-10-01.md).



\## Offen vom 28.09.2026 (Big2) — weiterhin gültig



\### G/Code und Daten



1\. Systematische Injektion je Hauptgruppe und Fehlertyp mit sauber eingefrorener Trainings-/Nachbarreferenz, ohne Nachtraining: Recall@5/10 und Zeilen-/Produktrang mit Nennern, Falschalarmrate an unveränderten Produkten und Abdeckung unbekannter Werte. Vier Handinjektionen waren ein eigener produktionsnaher Versuch, keine Recall-Schätzung.

2\. Exakte Bewertbarkeitsnenner, Fold-/Produktzuordnung, Modellmanifest und Referenzdaten des jüngsten Big2-Laufs für C sichern; der sichtbare GitHub-Lader belegt noch keine eingefrorene systematische Injektion.

3\. Mehrfachblöcke produktweise zusammenfassen, aber strukturell getrennt lassen; bei zusätzlicher gleicher Positionszeile die Grundreihenfolge Positionszahl vor Menge prüfen.

4\. Warum zeigt der Tiefe-1-Pilot keine Null-/Negativmengen, während frühere Dokumentation Nullmengen erwähnt? Roh-/Export-/Cleaning-/Hauptgruppen-/Tiefenwirkung auseinanderhalten.

5\. Big1s alte Trainings-/Ladezelle und Widerspruch zum Merkmal `kleinere\_gruppe` vor Abgabe separat prüfen. Kein Big1-Umbau als Nebenwirkung der Big2-Evaluation.



\### Fachbereich



\- Von G intern zuordenbare Kandidaten prüfen: der gemischte GJL121-Block, ein einzelner Federfall, zwei Produkte mit möglicherweise zusätzlicher Position und die abweichende Kontaktbestückung. Antworten mit Datum, Scope und Begründung als Labels speichern; keine pauschalen Ausnahmefilter ableiten.

\- Der bestätigte Big2-Halbierungsblock und der bestätigte Big1-Spitzenfall sind zwei getrennte Nutzennachweise; weitere Labels sind erforderlich.



\### C / Arbeit



\- Methoden-/Datenstand und Versuchskette mit Quellenstatus, Baseline 0,442/0,580 gegenüber Modell mit Nachbarn 0,0177/0,0317, Produktaggregation, vier Handinjektionen und \*\*noch offener\*\* systematischer Injektion dokumentieren.

\- Grenzen sichtbar machen: Tiefe 1, zwei Hauptgruppen, fehlende Nachbarmengen bei Unikaten, andere Familien in anderen Folds, fehlende reale Recall-Rate. Abgabeziel 16.10.2026.



Der \[Big2-Handoff](PA1\_BIG2\_Handoff\_2026-09-28.md) enthält die eigentlichen Kennzahlen, Quellen und den Prüfaufbau. Alle Fragen und „nächsten Schritte“ in den folgenden datierten Abschnitten sind historische Versuchsstände und keine erneuten Aufträge.



\## Historischer Fragenstand — Reverse-Ansatz, 14.09.2026



1\. Nach welcher fachlichen Regel gehören mehrere Materialnummernbasen zu derselben Komponentenart?

2\. Welche Teile des Kurztexts bezeichnen Typ, Seite, Bauform oder Ausführung und dürfen nicht weggeglättet werden?

3\. Welche Einbaukontexte sind wirklich vergleichbar; reicht für den ersten Pilot Tiefe 1?

4\. Warum meldete N für zwei bekannte Produkte 16/17 Auffälligkeiten, während A mit Leave-one-product-out 17/18 Mengenabweichungen findet?

5\. Gilt die vermutete Mengenregel auch für G03 sowie für seltene kleinere Gruppen, oder existieren legitime Ausnahmen?

6\. Wie werden ungeflaggt gebliebene Fälle stichprobenartig geprüft, damit nicht nur False Positives betrachtet werden?



Historischer Stand vom 09.09.2026: R1/R1.1 waren ausgewertet und der NMF-Pilot wurde damals gesichert. \*\*Nicht mehr aktueller Auftrag.\*\*



\## Jetzt am NMF-Pilot zu klären



| Frage | Status / nächste Prüfung |

|---|---|

| Wie lauten die konkreten Einträge von W und H? | Offen: Rohmatrizen wurden noch nicht als Dateien exportiert |

| Welche Komponenten prägen jedes Muster in H? | Offen: H mit Komponentenreihenfolge anzeigen und fachlich beschreiben |

| Ist sechs eine geeignete Musteranzahl? | Nicht entschieden; Kandidaten 2–15 vergleichen, Fehler allein reicht nicht |

| Bleiben relevante Gruppenpaare über verschiedene k stabil? | Offen: Rang-/Ähnlichkeitsstabilität über alle konvergierten Kandidaten prüfen |

| Bleibt das Ergebnis bei mehreren Zufallsstarts stabil? | Offen: Seeds und Parameter sichern, Wiederholungsläufe rechnen |

| Wie stark streuen Produkte innerhalb derselben kleineren Gruppe? | Offen: nicht nur Gruppenmittelwerte, sondern Verteilung der Produktprofile prüfen |

| Wie werden Gruppen mit 1–3 Produkten behandelt? | Sichtbar behalten, als datenarm markieren; keine automatische Zusammenführung |

| Sind VB6/VB6A, VBC6/VBC6A und B6/B6S fachlich plausible Kandidaten? | Mathematisches Indiz vorhanden, fachliche Bestätigung fehlt |

| Ist NMF besser als PCA oder Clustering? | Für diesen Pilot fachlich begründet, aber noch nicht empirisch gegen Baselines getestet |



Der vollständige Stand und Prüfplan stehen in \[08\_NMF\_PILOT\_STAND\_PRUEFPLAN.md](09-08\_NMF\_PILOT\_STAND\_PRUEFPLAN-2-.md). Der nächste Schritt ist die reproduzierbare Sicherung des Notebooklaufs einschließlich Parameter, `nmf\_ergebnisse`, W, H und Reihenfolgen.



\## Bereits geklärt



\- W hat eine Zeile je Produkt, nicht je kleinerer Gruppe. Beim 6-Muster-Modell ist die Form 772 × 6.

\- H hat eine Zeile je Muster und eine Spalte je Komponente. Beim 6-Muster-Modell ist die Form 6 × 103.

\- Die Tabelle mit 13 kleineren Gruppen entstand erst nach dem Training durch Normalisieren und Mitteln der W-Zeilen.

\- Sechs Muster wurden dem Modell vorgegeben; das Modell hat die Zahl sechs nicht selbst ausgewählt.

\- Kosinusähnlichkeit ist keine Wahrscheinlichkeit einer fachlich richtigen Zusammenführung.

\- Mehr als neun Muster sind trotz 13 kleinerer Gruppen möglich. Die Konvergenzwarnung bei k=10 hat damit nichts zu tun.

\- R1/R1.1 sind abgeschlossen: flache Struktur und gewöhnliche Positionsnummer reichen nicht zur allgemeinen Rollenbildung.



\## Erledigt, korrigiert oder abgelöst



\- Hauptgruppen nach ersten sechs Produkt-ID-Stellen, niemals vermischen: gesetzt und fachlich bestätigt laut Nutzer.

\- Begriffe und Projektziel: geklärt; kein erneuter Begriff „Einzelstückliste“.

\- Databricks-Zugang: Nutzer bestätigt vollen Zugang. Plattformwahl ist kein Hindernis für das Verständnis.

\- J-Master und Gruppen-Clustering erst erstellen: inzwischen ausgeführt; die früheren To-dos sind abgelöst.

\- „19 bestätigte Fehler“: nicht gesichert; 19 Zeilen versus später 17 Fälle bleibt historische Datenfrage.

\- Rollen über gleiches Erstwort: Heuristik, kein fachlicher Nachweis.

\- R0-Felder verfügbar?: durch N beantwortet; aktuelle Zahlen in 03. Keine unabhängige Vollwiederholung bei A.

\- Alle späteren Methoden auf einmal beauftragen: vom Nutzer abgelehnt; ein begrenzter Schritt nach dem anderen.



\## Weitere offene Punkte, ohne R1 damit zu überladen



Ursache der durchgehend 100 betragenden `alt\_wahrsch` bei Alternativzeilen; historische Mengen-/Spaltenfragen; Ns abweichende Gruppenzahlen in der Schwellenanalyse; Präzision der ML-Ränge und Definition `ml\_vs\_j`; Grenzen der Maskierung; unabhängige fachliche Bewertung und spätere Anomalieerkennung. Diese Punkte bleiben erhalten, sind aber kein neuer Sammelauftrag.



\## Historisches Fragenregister mit ursprünglichen Kennungen



Das folgende Register bewahrt Antworten, Quellen und Kennungen aus dem Stand bis 04.09.2026. „Offen“, „Go fehlt“ und „nächster Schritt“ darin sind \*\*damalige Zustände\*\*, keine erneuten Freigabeanforderungen. Die obige Liste enthält die aktuellen Statuskorrekturen. Nicht jede historische Frage ist erledigt; ohne spätere Antwort bleibt sie im Hintergrund offen, statt R1 zu blockieren.



<details>

<summary>Historischer Wissensstand bis 04.09.2026 (zum Nachschlagen)</summary>



\## FRAGEN



> Struktur nach Vorgabe des Bearbeiters: \*\*FRAGEN AI\*\* (selbst klärbar) / \*\*FRAGEN MENSCH\*\* (braucht Fachseite) / \*\*TO DO\*\*.

> Beantwortete Fragen bleiben \*\*mit Antwort\*\* stehen — sonst geht das Wissen verloren.

> Unternehmensneutral formuliert.



\---



\## FRAGEN AI



\### Offen



| # | Frage | Anmerkung |

|---|---|---|

| A1 | \*\*Kommt Menge = 0 nur bei Stückgut vor oder bei allen Mengeneinheiten?\*\* | Ausdrücklich vom Bearbeiter gefragt. In einer Minute am Export prüfbar. \*\*Go fehlt noch.\*\* |

| A2 | Domänentexte zur Sonderbeschaffung am System nachziehen | Abfrage lief zweimal in einen HTTP-403; tritt sporadisch auf, Wiederholung hilft |

| A3 | Was steht in den \*\*drei nie ausgewerteten Blättern\*\* der Arbeitsmappe (Selektionsblatt, Erzeugnisliste, Änderungsblatt 2017–2019)? | könnte relevant oder Altlast sein |

| A4 | Die \*\*drei Selbsttests zur Spaltenidentität\*\* durchführen | siehe `03\_DATENSATZ.md`, letzter Abschnitt |

| A5 | \*\*Nummernkreise neu und einfacher erklären\*\* | Meine Erklärung ist \*\*nicht angekommen\*\*. Mein Fehler, nicht der des Bearbeiters. |

| A6 | Was bedeutet „buchbar"? | Mein Begriff, wurde als unklar markiert. Muss definiert werden. |

| A7 | Was war mit „Phase 3" gemeint? | Gemeint war: den Buchungsweg einmal von einem Menschen erklären lassen. Muss klarer formuliert werden. |

| A8 | Rechte Spalten am System gegen echte Daten verifizieren | macht aus der Rekonstruktion belegtes Wissen |



\### Beantwortet



| Frage | Antwort |

|---|---|

| Kommt das Formatproblem von der Formatumwandlung? | \*\*Nein.\*\* Der Report hatte 2019 nur den alten Listendruck. Die Option für tabulatorgetrennten Export kam erst 2024. \*\*Ein neuer Export löst es.\*\* |

| Warum enthält der Export nur zwei Positionstypen? | Hilfs- und Textpositionen wurden beim Export \*\*ausgeblendet\*\* (Häkchen im Selektionsbild). |

| Woher kommen die \~5.300 Nullmengen? | \*\*Alternativpositionen mit Einsatzwahrscheinlichkeit 0.\*\* Das Ausblenden von Alternativpositionen war \*\*nicht\*\* aktiv. \*\*Kein Fehler.\*\* |

| Warum haben zwei Alternativen unterschiedliche gewichtete Mengen? | Sie liegen in derselben Alternativgruppe. Die Einsatzwahrscheinlichkeit verteilt 100 % innerhalb der Gruppe, die gewichtete Menge ist `Menge × EWAHR/100`. \*\*Und es ist eine Eigenschaft der Position, nicht des Materials\*\* — dasselbe Teil hat in anderen Stücklisten den umgekehrten Wert. |

| Wie heißen die Spalten wirklich? | \*\*36 Felder\*\*, Quelle ist die Ausgabestruktur des Reports im Dictionary. Vollständige Liste in `03\_DATENSATZ.md`. |

| Eine oder zwei Sonderbeschaffungs-Spalten? | \*\*Zwei\*\* — eine aus der Stücklistenposition, eine aus dem Materialstamm. \*\*Schüttgut ebenfalls zweimal.\*\* |

| Wie heißt die Selektionsvariante? | Der Bearbeiter hatte einen Buchstaben falsch notiert; der korrekte Name steht im System. |

| Was bedeutet „Mont" in den Notizen? | \*\*Montage.\*\* Steht als Kommentar im Quellcode des Reports. |

| Nutzt der Standort Variantenkonfiguration? | \*\*Nein\*\* — nur eine Handvoll konfigurierbarer Materialien gegen Millionen diskreter. Es liegt \*\*Modell B\*\* vor (siehe `02\_SAP\_WISSEN.md`). |

| Ist retrograde Entnahme relevant? | \*\*Ja, bei rund der Hälfte\*\* aller Material/Werk-Sätze aktiv. Regelfall. |

| Welches System liefert den Export? | \*\*Das operative System\*\* (D8). Bei Widersprüchen gilt der Export, nicht das Testsystem. |



\---



\## FRAGEN MENSCH



\### Kritisch — blockieren die Arbeit



| # | Frage | Warum kritisch |

|---|---|---|

| \*\*M1\*\* | \*\*Was ist eine Baugruppe in der Stückliste?\*\* | ⚠️ Der Bearbeiter hat das erklärt und wurde \*\*schief angeschaut\*\*. Meine Behauptung „eindeutig ableitbar" ist \*\*zurückgezogen\*\*. Ohne Antwort steht die ganze Baumlogik auf wackligem Grund. |

| \*\*M2\*\* | \*\*Zielsatz schriftlich bestätigen lassen\*\* | \*„Warum werden andere Sachen gebucht als in der Tabelle stehen?"\* — ein Satz vom Betreuer oder Ansprechpartner. Der letzte Vortrag lief flach, weil das nicht festgenagelt war. |

| \*\*M3\*\* | \*\*Welche der drei Mengenspalten steht im Export?\*\* | Betrifft D1 direkt und erklärt den ungelösten Widerspruch zu den Nullmengen. |

| \*\*M4\*\* | \*\*Bezieht sich die Menge auf das direkte Überteil oder auf das Fertiggerät?\*\* | D1 setzt „wie sie dasteht" als Arbeitsentscheidung; fachlich ist es offen. Blockiert Mengenauswertung über mehrere Stufen. |

| \*\*M5\*\* | \*\*Kann ein neuer, sauberer Export erzeugt werden?\*\* | ALV-Export statt Listendruck → \*\*36 saubere Spalten statt 27 verschmolzener.\*\* Kleinster Aufwand, größte Wirkung. |



\### Fachlich — Alternativpositionen



| # | Frage |

|---|---|

| M6 | Was bedeutet die \*\*Alternativposition-Strategie\*\*, Werte `1` und `2`? |

| M7 | Warum gibt es \*\*mehrere Alternativgruppen\*\* in einer Stückliste? |

| M8 | Warum steht bei manchen Alternativen die \*\*erste Wahl auf 0 %\*\* und die zweite auf 100 %? Wird die erste Wahl nicht mehr benutzt? |

| M9 | Sollen Alternativpositionen im Export überhaupt mitkommen, oder besser an der Quelle ausblenden? (R11) |



\### Fachlich — Dummys und Cleaning



| # | Frage |

|---|---|

| M10 | Sonderbeschaffungs-Werte bestätigen: \*\*was ist der zweite Dummy-Wert genau?\*\* |

| M11 | Bei „Dummy raus" — \*\*beide\*\* Sonderbeschaffungs-Spalten oder eine bestimmte? Der Bearbeiter sagte „egal welche"; zur Sicherheit bestätigen. |

| M12 | Sollen \*\*Informationspositionen\*\* raus? (R8 — ich habe sie bisher gefiltert, \*\*ohne Entscheidung\*\*) |

| M13 | Soll \*\*Schüttgut\*\* raus? (R9) |

| M14 | \*\*Stichtag\*\* für das Gültig-ab-Datum setzen? Stände von 2001 neben 2019. (R10) |



\### Prozess und Buchungen



| # | Frage |

|---|---|

| M15 | \*\*Wie kommt eine Buchung zustande?\*\* Retrograde Entnahme, Rückmeldung, Nachbuchung — einmal komplett erklären lassen. |

| M16 | \*\*Wer korrigiert, wann und warum?\*\* |

| M17 | \*\*Wie oft und wie stark weichen Buchungen ab?\*\* Größenordnung genügt. Der Bearbeiter: \*„keine Ahnung, aber es passiert, und das ist das Entscheidende."\* |

| M18 | \*\*Was kostet ein Fehler?\*\* Materialwert, Inventurdifferenz, Nacharbeit. Braucht die Arbeit für die Einleitung — und rechtfertigt das Projekt. |

| M19 | \*\*Wie validieren wir den Soll- und Ist-Prozess?\*\* (eigene Formulierung des Bearbeiters) |

| M20 | Gibt es bereits Auswertungen zu \*\*Inventurdifferenzen\*\* am Standort? |



\### Fehlerbild und Validierung



| # | Frage |

|---|---|

| M21 | \*\*Gibt es weitere bestätigte Fehlerfälle\*\* außer den 19 geprüften? Die sind alle vom selben Typ. |

| M22 | \*\*Wer hat die 19 gefunden und mit welcher Methode?\*\* Waren es echte Fehler oder Fehlalarme? |

| M23 | Welche Fehlerarten kommen real vor? Menge falsch, Teil zu viel, Teil fehlt, falsche Variante, falscher Positionstyp, veralteter Gültigkeitsstand? |

| M24 | \*\*10 Rollen aus der Ausgabe\*\* der Montage zeigen: „Sind das austauschbare Ausführungen desselben Bauteils?" → Trefferquote |



\### Struktur und Systematik



| # | Frage |

|---|---|

| M25 | \*\*Gibt es in SAP schon ein Feld für Baureihe / Produktgruppe?\*\* Produkthierarchie (`PRDHA`), Warengruppe (`MATKL`), Klassifizierung, ein kundeneigenes Feld? \*\*Wenn ja, wäre das Gruppierungsproblem gelöst\*\* und wir müssten keine Textanalyse rechtfertigen. |

| M26 | \*\*Ist die Materialnummer systematisch aufgebaut? Gibt es eine Dokumentation der Systematik?\*\* Bestätigen: codiert ein Buchstabe die Objektart, ist ein Zahlensuffix für „Basisteile" reserviert, codieren mittlere Ziffernblöcke Teileklassen? |

| M27 | Gilt „Nummernkreise nicht analog aufgebaut" nur für \*\*Bauteile\*\* oder auch für \*\*Erzeugnisse\*\*? Bei Erzeugnissen wirkt der Stamm systematisch — mit einem gefundenen Gegenbeispiel. |

| M28 | Ist der \*\*Materialkurztext nach Regeln gepflegt\*\*? Es wurde ein Kurztext mit fehlendem Anfangsbuchstaben gefunden. Wie häufig sind solche Tippfehler? |

| M29 | Was ist das \*\*Klassifizierungsmerkmal zur Bauteilart\*\*, wer pflegt es, \*\*warum nur 19 % gefüllt?\*\* Wäre der billigste Weg zur Substitutionserkennung. |

| M30 | Dürfen Geräte derselben Baureihe \*\*legitim stark unterschiedliche\*\* Stücklisten haben? Gibt es ein Kennzeichen, das kundenspezifische Varianten markiert? |



\### Organisatorisch



| # | Frage |

|---|---|

| M31 | Wer schaut die Kandidatenliste an, wie viele Einträge pro Woche sind zumutbar? |

| M32 | Was ist schlimmer — einen Fehler übersehen oder Fehlalarm? \*\*Bestimmt die Schwellwerte.\*\* |

| M33 | Soll das Ergebnis später in SAP laufen oder als separates Tool? (nur für den Ausblick) |

| M34 | Kann das \*\*operative System\*\* als System in der Arbeitsumgebung angebunden werden? Der Bearbeiter hat Zugriff, die KI nicht. |

| M35 | Zitierstil mit dem wissenschaftlichen Betreuer klären; ist ein \*\*Sperrvermerk\*\* nötig? |



\---



\## TO DO



| # | Aufgabe | Wer |

|---|---|---|

| T1 | Sauberen Export ziehen bzw. Formatierung in Ordnung bringen | Bearbeiter |

| T2 | \~\~Prüfen, ob die Formatumwandlung schuld war\~\~ \*\*erledigt: nein, es war der Export selbst\*\* | – |

| T3 | Die Felder durchgehen, die entscheiden \*\*ob\*\* eine Position gebucht wird — \*\*in Ruhe\*\* | beide |

| T4 | Phase 3 und Phase 4 inhaltlich klären | beide |

| T5 | \*\*Phasen neu und feiner nummerieren\*\* — \*„Phase 5 ist in der Realität eher Phase 10"\* | beide |

| T6 | \*\*ML-Plattform (Databricks o. ä.) prüfen\*\* statt nur lokales Python | beide |

| T7 | \*\*Bereinigungsskript bauen\*\*, sobald die Export-Frage entschieden ist | KI, nach Go |

| T8 | Rollenerkennung \*\*je Baureihe\*\* statt global | KI, nach Go |

| T9 | Bug beheben: Filter vor Baumbau kostet \~5 % der Zeilen | KI, nach Go |

| T10 | Skript 2 mit „alle Stufen" testen — eine Zeile, nie ausprobiert | KI, nach Go |

| T11 | \*\*Diese Dokumentation unternehmensneutral halten\*\* — bei jedem Update prüfen | KI, laufend |





\---



\## UPDATE 03.09.2026 — AKTUELL OFFEN



> Dieser Block ist aktueller als ältere Einträge oben, wenn sie sich widersprechen.



\### Verständnis / Phase 1 — ZUERST KLÄREN



| # | Frage |

|---|---|

| P1 | Was genau ist im aktuellen Clean-Datensatz ein Parent, Child und eine Parent-Child-Einheit? |

| P2 | Wann darf diese technische Einheit fachlich „Baugruppe“ genannt werden? |

| P3 | Was ist im Core ein Slot; wie wirken Positionsnummer und Alternativgruppe (D9) zusammen? |

| P4 | Was ist eine Vergleichsgruppe / ein „Topf“ und ist „Produktgruppe“ dafür überhaupt der richtige Begriff? |

| P5 | Welche Analyseeinheit ist sinnvoll: Erzeugnis, Parent-Child-Einheit, Slot oder eine Hierarchie? |

| P6 | Was bedeutet Pattern Matching in diesem Projekt methodisch? |

| P7 | Welche Rolle spielen später Ähnlichkeitsmaße, Clustering, Association Rules und Embeddings? |



\*\*Arbeitsregel:\*\* Diese Fragen zuerst mit dem Bearbeiter durcharbeiten. Noch kein neuer Modellcode.



\### Daten-/Messfragen aus der ersten Phase-1-Vermessung



| # | Frage |

|---|---|

| P8 | Wie genau wurde in Ns Messung „derselbe Parent“ definiert? |

| P9 | Sind die 26 variablen Parents wirklich nur zwei Formen — und wodurch unterscheiden sich Form A/B konkret? |

| P10 | Warum entstehen die zwei Formen: Variante, Gültigkeitsstand, Alternativposition, Exporteffekt oder etwas anderes? |

| P11 | Sind 66 % exakt einzigartige Wurzel-Zusammensetzungen trotzdem untereinander stark ähnlich? |

| P12 | Wurden die 17.402 / 5.846 Slots exakt nach der Core-D9-Logik gebildet oder vereinfacht über Parent+Positionsnummer? |

| P13 | Wie stark verändert die Dummy-Auflösung die für Phase 1 sichtbare Ebenenstruktur? Ohne R5/R6 zu ändern: welche ursprüngliche Strukturinformation bleibt über `eltern\_orig`/Rohdaten verfügbar? |



\### Legacy-Cleaner / neuer Export



| # | Frage |

|---|---|

| P14 | Wie sieht ein aktueller Live-Export tatsächlich aus? Welche Legacy-Reparaturen sind dort noch nötig? |

| P15 | Tritt der verrutschte-Stufe-Effekt im neuen Export überhaupt noch auf? Wenn ja: welche konservative generische Recovery ist sicher? |

| P16 | `teil\_id`: Welche Identitätsdefinition soll Phase 1 tatsächlich verwenden? Materialnummer, Text-ID, Rolle oder mehrere Sichten parallel? |

| P17 | Mengenparser: treten in neuen Exporten Textzahlen/Dezimalstrings auf oder weiterhin echte numerische Excel-Werte? |



\### Diskrepanzen-/Validierungsblatt



| # | Frage |

|---|---|

| P18 | Warum sind aktuell 17 inhaltliche Fälle gemessen, während ältere Doku von 19 Zeilen/Fällen spricht? |

| P19 | Sind die Diskrepanzen bestätigte Stücklistenfehler oder primär Mapping-/Referenzabweichungen zwischen zwei Systemen? |

| P20 | Welche Quelle ist in den vergleichbaren Fällen fachlich korrekt? Fachseite bestätigen lassen. |



\### Erledigte alte ToDos



\- Bereinigungsskript existiert inzwischen (`bom\_core`).

\- Filter-vor-Baum-Bug ist im Core-Grundprinzip behoben.

\- R8 ist nicht mehr offen: freigegebener Stand = nur `L`.

\- R9: Schüttgut bleibt.

\- R12: Dubletten nur zählen.

\- Der alte Listendruck ist als Ursache des Formatproblems geklärt.



\---



\## UPDATE 04.09.2026 — AKTUELLER FRAGENSTAND



> Dieser Block ersetzt bei Widersprüchen die Arbeitsreihenfolge vom 03.09.2026.

> Die alten Fragen bleiben als Entwicklungsgeschichte erhalten.



\### Seit 03.09 geklärt oder eingegrenzt



| Früher | Aktueller Stand |

|---|---|

| P1–P7: erst alle Begriffe klären, noch keinen Modellcode bauen | \*\*Als Lernblock abgeschlossen bzw. durch den praktischen J-Ansatz ersetzt.\*\* Der Bearbeiter versteht die aktuelle Vergleichsidee; weitere Begriffe werden am konkreten Ergebnis erklärt. |

| P5: Analyseeinheit offen | Für die aktuelle Gruppierungsphase sind es \*\*vollständige Erzeugnisse bzw. aggregierte Originaltypen innerhalb einer 6-Zeichen-Hauptgruppe\*\*. Spätere Fehlerhinweise können auf Komponenten-/Positionszeilen zurückführen. |

| P16: Teileidentität offen | Für Phase 1 gilt \*\*Materialnummer = Komponentenidentität\*\*. Kurztexte dienen der Darstellung und vorläufigen Teileartbildung, nicht dem Zusammenwerfen verschiedener Materialien. |

| „20 gleiche Komponenten“ beim bekannten Typpaar | War \*\*nur ein fachliches Erklärbeispiel\*\*, keine gemessene Zahl und keine Regel. |

| Noch kein praktischer Versuch | Der J-Prototyp läuft. Aktuell fehlen vor allem vollständige Evidenzdarstellung, Fachvalidierung und ML-Vergleich. |



\### J-Ansatz — Fragen an N / aus den Daten klärbar



| # | Frage | Erwarteter Beleg |

|---|---|---|

| J1 | Warum umfasst die maskierte Paaransicht 1.656 statt 1.692 Erzeugnisse? Welche 36 Fälle fehlen und warum? | Coverage-Tabelle je Erzeugnis/Typ mit Ausschlussgrund |

| J2 | Stimmt beim fachlichen Referenzpaar die 50-%-Kernüberlappung 0,9091 oder 1,0? | Neu berechnete interne Zeile plus konkrete Komponenten |

| J3 | Welche konkreten Komponenten wurden als Gegenstücke gepaart, und warum? | Materialnummer, Kurztext, n/N beider Typen, Paargrund |

| J4 | Welche konkreten „echten Trennteile“ entscheiden jedes akzeptierte oder knapp abgelehnte Paar? | Materialnummer, Kurztext, n/N und Entscheidungswirkung |

| J5 | Wie empfindlich sind Gruppen gegenüber den Schwellen regelmäßig ≥50 %, praktisch nie ≤10 % und maximalen Trennteilen? | kleine Sensitivitätstabelle, nicht nur ein Endwert |

| J6 | Erzeugen interne und maskierte Ausgabe dasselbe Schema und exakt dieselben aggregierten Zahlen? | automatischer Gleichheitstest ohne Klartexte/IDs |

| J7 | Ist die Gruppenzuordnung reproduzierbar und aus einer einzigen Masterquelle abgeleitet? | Code-/Parameterstand und Ableitung CSV→HTML/Excel |



\### J-Ansatz — Fragen an den Fachbereich



| # | Frage | Warum wichtig |

|---|---|---|

| J8 | Sind alle sieben datengetrieben zusammengelegten Typgruppen fachlich korrekt? | Das sind beschaffbare Gruppierungslabels und damit die wichtigste Validierung der Regel. |

| J9 | Ist die vorläufige Teileart „erstes Wort des Kurztexts“ fachlich tragfähig? | Tippfehler und uneinheitliche Benennung können falsche Gegenstücke erzeugen. |

| J10 | Welche der vorgeschlagenen Komponentenpaare erfüllen tatsächlich dieselbe Funktion? | Verhindert, dass nur ähnlich klingende Teile als Gegenstücke gelten. |

| J11 | Sind aktuell getrennte Varianten fachlich wirklich getrennte Vergleichsgruppen? | Liefert bewusst schwierige Negativbeispiele, nicht nur bestätigte Zusammenlegungen. |

| J12 | Gilt die 6-Zeichen-Systematik auch außerhalb des betrachteten Produktbereichs? | Der J-Ansatz ist aktuell fachlich gestützt, aber noch nicht universell. |



\### Datenfragen mit Auswirkung auf spätere Fehlerhinweise



| # | Frage | Status |

|---|---|---|

| DQ1 | Wie sollen Alternativpositionen berücksichtigt werden? N meldet 16.129 Zeilen mit Alternativkürzel; „in BOM gelistet“ kann von „tatsächlich gewählt“ abweichen. | offen |

| DQ2 | Wie werden Mengen über Ebenen fachlich interpretiert? | D1 bleibt Arbeitsentscheidung; fachlich offen |

| DQ3 | Gibt es einen aktuellen Live-Export und enthält er die Legacy-Stufenverschiebung noch? | offen; Generalisierungstest nötig |

| DQ4 | Sind die 17 Diskrepanzfälle echte BOM-Fehler, Mapping-Abweichungen oder gemischt? | offen; nicht als Fehlerlabels verwenden |

| DQ5 | Warum wurden historisch 19, später aber 17 Fälle berichtet? | offen |



\### Statistik / ML



| # | Frage | Hinweis |

|---|---|---|

| ML1 | Welche Distanz und welches Linkage bilden Typ-Komponenten-Häufigkeitsvektoren verständlich ab? | Zunächst mehrere kleine Vergleichsvarianten mit Dendrogramm/Distanzmatrix zeigen. |

| ML2 | Wie stimmen J-Gruppen, ML-Struktur und Fachurteile überein? | ML ist ein zweiter Vorschlag, nicht die Wahrheit. |

| ML3 | Wie wird validiert, ohne die 89 abhängigen Typpaare zufällig in Train/Test zu teilen? | Auf Typ-/Hauptgruppenebene und mit Fachlabels prüfen. |

| ML4 | Woher kommen bestätigte Fehlerlabels für die spätere Anomalieerkennung? | Noch offen; ohne Labels keine gemessene Fehlererkennungsleistung. |



\### Dokumentation / Praxisarbeit



| # | Frage / Aufgabe | Wer |

|---|---|---|

| C1 | Chronologisches Versuchsregister aus den bisherigen Läufen herstellen und nach jedem neuen Lauf fortführen | C mit Belegen von N/A |

| C2 | Kapitelentwurf parallel pflegen: Problem, Ziel/Scope, Daten, Cleaning/Baum, Versuchshistorie, J-Ansatz, später ML und Ergebnisse | C + Bearbeiter |

| C3 | Für jeden Befund Status stabil/vorläufig/widerlegt/offen und Belegpfad festhalten | alle |

| C4 | Zitierstil und Sperrvermerk klären | Bearbeiter / Hochschule / Betrieb |

| C5 | Für das Weekly eine zentrale Tabelle und wenige erklärbare Abbildungen statt verteilter CSVs bereitstellen | N; A prüft Verständlichkeit |



\### Nächste Reihenfolge



1\. Zentrale J-Master-Tabelle erstellen und J1–J7 intern beantworten.

2\. J8–J11 mit dem Fachbereich prüfen.

3\. ML1–ML3 als Gruppierungsvergleich ausführen.

4\. Stabile Ergebnisse unmittelbar an C für Register und Kapitelentwurf geben.

5\. Danach Fehlerpriorisierung; nicht vorher behaupten, dass Gruppenabweichung ein

&#x20;  bestätigter Stücklistenfehler ist.



</details>

