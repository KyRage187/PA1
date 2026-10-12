\# 04 — ENTSCHEIDUNGEN UND REGELN



\## Aktuelle Entscheidungen — 03.10.2026 (Nutzerentscheidungen, Fortsetzung ohne neue D-Nummern)



1\. \*\*Fachbereichs-Ausgabe = eine Tabelle, feste Spaltenreihenfolge:\*\* Konfidenz, Produktname, Produktnummer, Teilename, Teilenummer, Befund, Beleg, Verglichen mit. Keine Block-, Spannungs-, Gewichtungs- oder Hauptgrund-Spalten mehr. Nutzerentscheidung, zweifach korrigiert (v2 zu viele Spalten/Tabellen → v3 Vergleichsprodukte fälschlich ganz entfernt → v4 Vergleichsprodukte-Spalte wieder ergänzt).

2\. \*\*Bündelung erlaubt und umgesetzt:\*\* Mehrere Einzelmeldungen desselben Produkts dürfen zu einer Zeile zusammengefasst werden, wenn sie demselben Muster folgen (Strukturabweichung, Variante, Baugruppe fehlt). Konfidenz der Bündelzeile = Maximum der Einzelkonfidenzen, \*\*nicht neu berechnet.\*\*

3\. \*\*Beleg-Konfidenz-Verrechnung vorerst nicht umsetzen\*\* (Punkt 4 des 5-Punkte-Plans): Modellwert × Belegfaktor wäre möglich, Nutzer hat das als gute Idee bezeichnet, aber ausdrücklich zurückgestellt („nein erstmal nicht aber ist gut finde ich nice idee“).

4\. \*\*Vergleichsprodukte nach Typbezeichnung gewichten (Punkt 1) ist NICHT freigegeben.\*\* Grund: greift in Cmd 5/6 (Training/Nachbarauswahl) ein und würde alle bestehenden Scores verändern. Erst nach Rückmeldung zu Cmd 8 v4 erneut vorlegen.

5\. \*\*Kalibrierung im eigentlichen ML-Sinn erst nach Fachbereichslabels.\*\* Vorher nur Ranking/Präzision verbessern (Bündelung, ggf. später Typbezeichnung). Begriffsklärung vom Nutzer selbst initiiert.

6\. \*\*`komponente\_basis`-Regex bleibt unverändert\*\*, auch wenn sie an ein bestimmtes Teilenummernschema gebunden ist — ausdrücklicher Nutzerwunsch, dort nichts zu ändern.

7\. \*\*`HAUPTGRUPPEN`-Liste muss vor Multi-Stücklisten-Test generalisiert werden\*\* — als Vorgabe für den nächsten Schritt festgehalten, noch nicht umgesetzt.

8\. \*\*Arbeitsweise unverändert:\*\* komplette Ersetzungszellen mit Cmd-Nummer, keine Suchanweisungen, keine erneuten Gesamterklärungen, Rückfragen bei Unklarheit statt Annahmen.



\## Aktuelle Big3-Entscheidungen — 01.10.2026 (Nutzerentscheidungen, ohne neue D-Nummern)



1\. \*\*Universalität ist oberste Regel:\*\* Jedes Modell muss auf neuen, nie gesehenen Stücklisten laufen. Nur `bom\_core.clean\_bom()` kennt das Exportformat. Modellcode liest ausschließlich `sauber`. Der Big3-Excel-Parser (Läufe 1–4) ist deshalb \*\*verworfen\*\*; sein Lauf 3 bleibt nur als Vergleichswert dokumentiert.

2\. \*\*Big3 ist ein eigenes Modell\*\* mit denselben Zeilen, Folds, Gewichten und Hyperparametern wie Big1 V2 plus 14 Strukturmerkmalen. \*\*Basis ist V2\*\*, nicht V1 (V1-Basis ließ den bestätigten Spulenfall auf 0,25 fallen). Stacking (`v2+big1`) ist ein späterer Test, kein Standard.

3\. \*\*Keine Anpassung an Einzelfälle\*\* (Joch-Nachbarbildung ausdrücklich untersagt).

4\. \*\*Ausgeschlossen bleiben:\*\* Teilenamen/Kurztexte als Regel, Typlisten, feste Sollstrukturen, Excel-Spaltenpositionen. Verwendet werden nur Cleaner-Felder: `stufe, tiefe, eltern, node, slot, alt\_gruppe, alt\_wahrsch, menge`.

5\. \*\*Labels:\*\* Einziger bestätigter Fall ist die Spule 60 V (Big1-Spitzenfall). Alle anderen Big1/Big3-Fälle sind unbestätigt. Auffälligkeit ist ein Rang, keine Fehlerwahrscheinlichkeit.

6\. \*\*Stopppunkt:\*\* Nach „vorzeigbar“ (01.10.) keine weiteren Big3-Änderungen vor der Weekly-Rückmeldung. Nächster Schritt ist das gemeinsame Notebook mit einer Liste (GJL121 + GJL131), nicht durch G.

7\. \*\*Arbeitsweise:\*\* Komplette Ersetzungszellen mit Cmd-Nummer; vorausgesetzte Variablen am Zellanfang; kein erneutes Ausholen; Handoff nur auf Anweisung.



Belege: \[G\_HANDOUT](G\_HANDOUT\_Big3\_2026-10-01.md) Abschnitte 4 und 7.



\## Historische Big1/Big2-Entscheidungen — 28.09.2026 (Big2-Teile gültig; „Big3 später“ abgelöst)



1\. Big1 V2 bleibt eingefrorener Anwesenheits-Benchmark. Big2 behandelt die \*\*Menge vorhandener exakter Komponenten\*\* und ist für den nächsten Pilot mit fünf datengetriebenen Nachbarschaftsmerkmalen und nachgelagerter Produktaggregation festgelegt. Big3/Komponentengruppen sind eigene spätere Schritte.

2\. Big2 wird für GJL121 und GJL131 derzeit mit \*\*getrennten\*\* Modellen je Hauptgruppe und produktgetrennten fünf Folds bewertet. Pooling ist offen, nicht als Standard freigegeben. Keine Produktnamen-Typenschlüssel, festen Sollmengen oder Sonderregel für einen bestätigten Fall.

3\. Als nächstes systematische Mengen-/Halbierungs-/Duplikat-Injektion mit geladenen Modellen und \*\*sauber eingefrorener Trainings- und Nachbarreferenz\*\*; den produktionsnahen Vier-Fall-Handversuch gesondert benennen. Ohne seine Auswertung keine weiteren Merkmale, Hauptgruppen oder Parameteränderungen.

4\. Komponenten- und Produktränge, Abdeckung und Falschalarmfälle mit Zähleinheiten ausweisen. `1 − p\_ist` ist ein Anomalierang, keine Wahrscheinlichkeit eines echten Fehlers. Fachurteile bleiben eigenständige Labels.

5\. Bestehende D-Kennungen werden hier nicht fortgesetzt: `D19–D27` und `D26–D34` wurden in der alten Datei teilweise mehrfach vergeben. Frühe Reverse-Vorgaben stehen unten als \*\*historischer Geltungsbereich\*\*, nicht als aktuelle Pflicht zur vorgeschalteten Komponentengruppierung.



Belege und Grenzen: \[Big2-Handoff](PA1\_BIG2\_Handoff\_2026-09-28.md).



\## Historische Reverse-Entscheidungen D19–D27 — 14.09.2026



| ID | Entscheidung | Status / Grenze |

|---|---|---|

| D19 | Aktiver Weg ist komponentenbasiert („Reverse“). | Nutzerentscheidung nach Weekly. |

| D20 | Schritt 1 ist die Bildung belastbarer Komponentengruppen; Produktverteilungen sind Schritt 2. | Keine Vorwegnahme von Schritt 2. |

| D21 | Komponentengruppen nutzen Materialnummernbasis \*\*und\*\* Kurztext; Fuzzy-Text ist nur Kandidatenfinder. | Kein globaler Textschwellenwert beschlossen. |

| D22 | Einbaukontext wird berücksichtigt; Tiefe allein genügt nicht. | Zunächst Tiefe 1, später exakter Eltern-/Kontextbezug. |

| D23 | NMF bleibt geparkt. | Nicht gescheitert oder instabil; kein nachgewiesener Zusatznutzen und keine begründbare Wahl von Variante/k. |

| D24 | G08 ist im alten Gruppenvergleich ein Sonderfall und darf keine allgemeine Schwelle kalibrieren. | Von N datenstrukturell belegt. |

| D25 | Kandidaten werden nicht als Fehler bezeichnet. | Fachbestätigung erforderlich. |

| D26 | Regelabdeckung, Zahl der Kandidaten und Validierung werden getrennt berichtet. | False-Negative-Rate ohne Labels nicht messbar. |

| D27 | Hauptgruppen bleiben über die ersten sechs Stellen getrennt; kleinere Gruppenregel bleibt vorerst unverändert. | Keine Spannungs-/Ausführungs-Unterteilung beschlossen. |



Historischer Stand: \*\*09.09.2026\*\*. Entscheidungen sind Arbeitsvorgaben mit Herkunft und Geltungsbereich, keine automatisch bewiesenen fachlichen Tatsachen. Neuere ausdrückliche Nutzerentscheidungen haben Vorrang vor älteren Notizen.



\## Bestehende Entscheidungen D1–D18, konsolidiert



| ID | Geltender Inhalt | Herkunft / Grenze |

|---|---|---|

| D1 | Mengenspalte wie geliefert übernehmen; keine Multiplikation mit dem übergeordneten Teil | Nutzerentscheidung. Bezug/Basismenge und Roh-/Gewichtungsstatus fachlich offen; R1 nutzt Menge nicht im Profil. |

| D2 | Nur ST; Menge 0 nicht pauschal entfernen oder als Fehler werten | Nutzerentscheidung; nicht jede Nullmenge ist als legitime Alternative bewiesen. |

| D3 | Wiederverwendbarer Algorithmus ist das Ziel | Frühere Aussage „Exportherkunft unwichtig“ war zu pauschal: Herkunft/Version bleiben für Reproduzierbarkeit nötig. |

| D4 | Keine auf konkrete Produktnamen oder Präfixwerte zugeschnittene Einmallösung | Generalisierungsziel. Die im Projekt gesetzte sechsstellige Hauptgruppierung bleibt eine fachliche Vorgabe. |

| D5 | Fachvalidierung über zuständige Fachseite / betrieblichen Betreuer | Nutzer: auch per E-Mail jederzeit, nicht nur im Weekly. Kein Sendeauftrag durch diese Notiz. |

| D6 | Dummykennzeichen 35/50 in beiden erkannten Sonderbeschaffungsfeldern entfernen; andere Beschaffungswerte behalten | Nutzerentscheidung im Projekt; keine pauschale Aussage, dass alle Schlüssel in jedem System gleich belegt sind. |

| D7 | Relative Projektpfade über `pathlib`, Ausgangspunkt Skript-/Projektverzeichnis | Nutzerentscheidung; reproduzierbare Startbefehle dokumentieren. |

| D8 | Export stammt aus operativem System; Testsystemdaten ersetzen ihn nicht | Nutzerentscheidung zur konkreten Datenquelle, keine Behauptung, Export sei fehlerfrei. |

| D9 | Alternativgruppe bildet innerhalb ihres ursprünglichen Zusammenhangs einen gemeinsamen Slot | Aktuelle Core-Logik; ohne Alternative Position, sonst technischer Fallback. Nicht gleiche Kürzel über Kontexte verbinden. |

| D10 | Gesamtes PA1-Projekt ist A; bestehende Wissensdateien sind dessen Gedächtnis | Nutzer mehrfach klargestellt; neue Chats sind neue A-Sitzungen. |

| D11 | Komponentenidentität für die Analyse über `Identnummer Teil` / `komponente` | Unterschiedliche Nummern bleiben erhalten. Text-ID und mögliche Rolle sind eigene Informationen. |

| D12 | J bleibt die fachlich motivierte Gruppierungsbaseline | Sechsstellige Hauptgruppen → kleinere Gruppen → Komponentenvergleich. Rollenentwicklung ergänzt die bislang unzureichende Wortheuristik. |

| D13 | Das Beispiel B6/B6S ist fachlich zusammengehörig | Nutzer/Fachanker. „20 gleiche Komponenten“ war nur ein Beispiel, keine Zahl/Schwelle. |

| D14 | Schrittweise praktische Versuche, sichtbare Ergebnisse und dokumentierte Fehlschläge | Nutzerentscheidung. Nicht sämtliche Kriterien vorab von ihm verlangen. |

| D15 | Ergebnisse je Verfahren aus einer nachvollziehbaren gemeinsamen Datenquelle berichten | J-Master wurde inzwischen geliefert. Keine parallele Neuberechnung im Bericht; keine riesige manuelle Tabelle für den Nutzer. |

| D16 | Relevante Versuche/Entscheidungen für C dokumentieren | Auch Hypothesen und Fehlschläge mit Status. Kein starres Wochenraster. |

| D17 | Bisheriges Clustering ist ein Vergleich neben J | Inzwischen ausgeführt; keine gelernte Komponentenrolle und kein Anomaliedetektor. |

| D18 | As Prüfungen maskierter Daten als solche kennzeichnen; N validiert Corporate-Bezug | Dateiprüfung, N-Bericht und Fachurteil nicht gleichsetzen. |



\## Ergänzungen D19–D25 aus diesem Chat



| ID | Entscheidung / Grenze | Begründung |

|---|---|---|

| D19 | \*\*Hauptgruppen nie mischen.\*\* Erste sechs Stellen beziehen sich auf die Produkt-Identnummer. | Fachlich bestätigt; nicht erneut als offene Grundsatzfrage behandeln. |

| D20 | Einheitliche Begriffe nach AGENTEN\_ROLLEN verwenden; „Einzelstückliste“ entfällt | Nutzerverständnis und gleiche Gesprächsebene sichern. |

| D21 | Drei Aufgaben: Rollen auf Komponentenebene, Zusammenführung kleinerer Gruppen, danach Anomalien | Nutzer bestätigt; jetziger Versuch ist Aufgabe 1. |

| D22 | R0, R1 und weitere Schritte getrennt beauftragen; kein LLM im aktuellen Rollenverfahren | Nutzer will kleine überprüfbare Schritte und Lernen aus Daten. |

| D23 | R1 verwendet bereinigten Aufbau mit `eltern`; `tiefe` ist nur Kontrollinformation | Erteilter R1-Auftrag; keine endgültige Wahl für alle künftigen Methoden. |

| D24 | In R1 Blöcke strukturell trennen und je Produkt dieselbe Information höchstens einmal zählen | Mehrfachblöcke dürfen keine künstliche Häufigkeit erzeugen. Unterschiedliche Blockinhalte nicht zu gemeinsamem Einbau erfinden. |

| D25 | R1 muss die Gefahr einer großen manuellen Prüfliste sichtbar bewerten | Nutzerbedenken; weder Top-K-Ausgabe noch ungeprüfte automatische Entscheidungen beweisen Entlastung. |



\## Ergänzungen D26–D34 aus R1/R1.1 und dem NMF-Piloten



| ID | Entscheidung / Grenze | Begründung und Status |

|---|---|---|

| D26 | R1 und R1.1 gelten als abgeschlossene begrenzte Rollenversuche; nicht erneut starten | Code, N-Berichte und maskierte Ausgaben liegen vor |

| D27 | Reine Eltern-/Nachbarschaftsstruktur und gewöhnliche Positionsnummer werden vorerst nicht weiter als automatische Rollenlösung ausgebaut | R1 zu breit; R1.1 kleiner, aber inhaltlich schlechter |

| D28 | Vorhandene `ALT:`-Slots dürfen als gepflegtes Rollenwissen beziehungsweise Kontrollanker genutzt werden, aber nicht auf ungepflegte Positionen verallgemeinert werden | 6/6 bekannte Alternativpaare teilen die Signatur; nur sechs verschiedene Paare |

| D29 | Der NMF-Pilot ist ein separater Gruppenvergleich, keine Fortsetzung des Rollenmodells | Er arbeitet auf binärer Komponentenpräsenz je Produkt und lernt keine funktionale Rolle |

| D30 | Im NMF-Training dürfen kleinere Gruppen, Gruppennamen und Kurztexte nicht als Merkmale eingehen | Die Gruppenzuordnung dient erst nach dem Training zur Aggregation und Bewertung |

| D31 | Das 6-Muster-Modell ist nur ein anschaulicher Snapshot; keine optimale Musteranzahl ist ausgewählt | Modelle mit 2–15 Mustern wurden gerechnet; Rekonstruktionsfehler sinkt erwartbar mit k |

| D32 | W enthält Produkte, nicht kleinere Gruppen. Eine Gruppenzeile entsteht erst durch Relativierung und Mittelung der W-Zeilen | Zentrale Verständniskorrektur des Bearbeiters |

| D33 | NMF-/Kosinusähnlichkeit ist keine Wahrscheinlichkeit und keine automatische Zusammenführungsentscheidung | Fachurteil und Stabilitätsprüfung fehlen |

| D34 | Vor weiterer Modellentwicklung zuerst den NMF-Lauf reproduzierbar sichern und H ansehen | Notebook, Parameter, rohe W/H und Reihenfolgen fehlen als dauerhafte Artefakte |



Der ursprüngliche R1-Auftrag lag in einem gesonderten historischen Artefakt und blieb unverändert. R1.1 wurde anschließend als genau ein Positionstest ausgeführt. Diese Passage begründet keine neue Rollenstufe, automatische Zusammenführung oder Anomaliefreigabe.



\## Cleaning-Regeln — eigene alte Nummerierung



\*\*Die Cleaning-Regeln R1/R2 sind nicht die Rollenversuche R0/R1.\*\* Zur eindeutigen Bezugnahme stets „Cleaning-R…“ oder „Rollenversuch R…“ nennen.



| Cleaning-ID | Geltender Stand |

|---|---|

| R1 | Wiederholte Kopfzeilen entfernen |

| R2 | Leerzeilen entfernen |

| R3 | Nur ST behalten |

| R4 | Nullmengen nicht pauschal entfernen |

| R5 / R6 | Dummy 50/35 in beiden erkannten Feldern entfernen, überlebende Kinder umhängen |

| R7 | Andere Beschaffungsschlüssel behalten |

| R8 | \*\*Nur L behalten\*\*; X entfernen. Früher offen, seit 03.09. freigegeben. |

| R9 | \*\*Schüttgut behalten\*\*. Früher offen, jetzt keine Schüttgutentfernung. |

| R10 | Umgang mit Stichtag/Gültigkeitsständen fachlich weiter offen |

| R11 | Keine pauschale Entfernung wegen Einsatzwahrscheinlichkeit 0; künftige Exportselektion separat klären |

| R12 | Dubletten nur zählen, nicht automatisch entfernen |



Reihenfolge: Daten und Originalreihenfolge erfassen → Baum vor fachlichen Filtern rekonstruieren → entfernbare Zeilen markieren → Kinder an nächsten überlebenden Vorfahren umhängen → markierte Zeilen entfernen → Tiefe neu berechnen. Ein früheres Skript verlor durch umgekehrte Reihenfolge rund 5 % der Zeilen; das ist kein weiterhin belegter Bug im aktuellen Core.



Bei Stufensprüngen nächster vorheriger Knoten mit \*\*kleinerer\*\* Stufe, nicht zwingend genau eine Stufe weniger. Stufenzahl lesen, nicht Sterne zählen. `node` identifiziert ein konkretes Vorkommen; identische Texte oder Materialnummern dürfen mehrfach vorhandene Elternvorkommen nicht verschmelzen.



Die 144 verrutschten Stufen des Legacy-Exports wurden für den Versandlauf gezielt vor dem Core rekonstruiert. Dadurch blieben 9 sonst gültige Positionen zusätzlich erhalten. „Unlesbare Stufe entfernen“ war keine fachliche Entscheidung. Keine allgemeine Regel „letzte Zahl im Kurztext ist die Stufe“ nachträglich erfinden.



\## R1 — abgeschlossener verbindlicher Umfang in Kurzform



| Information | Verwendung im erteilten Auftrag |

|---|---|

| Hauptgruppe | Harte Trennung aller Vergleiche |

| Produkt-ID und Block | Lokaler Zusammenhang und eindeutige Produktzählung; Produkt-ID nicht als Ähnlichkeitssignal |

| Komponenten-ID | Schlüssel einer Komponente; keine Nummernsemantik ableiten |

| `eltern` | Konkretes Elternvorkommen zur Rekonstruktion; aufgelöste Eltern-Komponenten-ID als Umgebungsmerkmal |

| Andere direkte Komponenten | Umgebungsmerkmale aus exakt demselben Elternvorkommen und Block |

| Produktwurzel | Mitführen; allein kein unterscheidendes Ähnlichkeitssignal |

| `tiefe` | Nur Kontrolle/Erklärung, nicht im Profil gewichtet |

| Kurztexte und kleinere Gruppe | Nur Darstellung, keine Berechnung der Rollenähnlichkeit |

| Positionsnummer, Menge, `stufe`, `eltern\_orig` | Keine direkten Merkmale des R1-Profils |

| `alt\_wahrsch` | Vollständig aus R1-Bewertung ausschließen |

| Bestehender `ALT:`-Slot / Alternativgruppe | Nur Ausnahme bei gemeinsamem Auftreten und nachgelagerte Kontrolle; kein Bonus/Trainingslabel |

| J-Entscheidungen / J-Gruppen | Nicht als Lerninformation oder Zielgröße verwenden |



Der bestehende Slot enthält technisch `eltern\_orig`. Seine Nutzung zur Alternativprüfung hebt nicht das Verbot auf, `eltern\_orig` als direktes Ähnlichkeitsmerkmal einzubauen. Die beiden Verwendungszwecke müssen im Code nachvollziehbar getrennt sein.



Ausgabe: vollständige technische CSV, kleine erklärbare Darstellung und Verteilung der Werte. IDF-artige datenbasierte Gewichtung und Kosinus; gemeinsames Auftreten separat. Kein LLM, neuronales Netz, endgültige Rollengruppen, Gruppenverschmelzung, Anomaliedetektor oder R2. Cleaner und J blieben fachlich unverändert. Das Ergebnis und der spätere Positionstest sind in 06/07 dokumentiert.



\## Offene Entscheidungen nicht in Regeln verwandeln



\- Wann Rollen fachlich gleichwertig sind, wird an Fällen iterativ gelernt/validiert.

\- Keine endgültige Transitivitäts-/Cliquenregel: ein historisches Kettenproblem ist bekannt, aber der neue Fall soll erst gezeigt werden. R1 bildet ohnehin keine Rollengruppen.

\- Keine feststehenden Automatikschwellen oder Mindestquote zur manuellen Entlastung; Qualität mitberücksichtigen.

\- Keine Zusage, dass das Rollenmodell nach diesem ersten Versuch zuverlässige Wahrscheinlichkeiten liefert.

\- Position wurde in R1.1 als einzelne Erweiterung getestet und als allgemeines Rollensignal verworfen. Menge, ursprünglicher Aufbau oder Texte bleiben mögliche spätere Einzelversuche, sind aber nicht beauftragt.

\- Databricks-Zugang ist gemeldet; kein Plattformwechsel beauftragt.



\## Allgemeine Umsetzungsregeln



`clean\_bom()` ist die gemeinsame Eingangsstufe; die menschenlesbare XLSX wird nicht als Ersatz zurückgelesen. Felder über Namen/Fingerprints erkennen, nicht blind über feste Spaltennummern. Originalwerte und Belegzeilen zur Rückverfolgung erhalten. Code, Datenstand, Parameter und Zähleinheiten nennen. Berichte rechnen Ergebnisse nicht separat nach.



Keine Geschäftsdaten aus vermeintlich maskierten Kurztexten in die Projektdokumentation kopieren. Arbeitsregeln zu Sprache und Freigaben stehen in \[AGENTEN\_ROLLEN](13-AGENTEN\_ROLLEN-5-%282%29.md). Die aktuelle Übergabe pflegt Projektwissen; die damalige R1-Freigabe ist ein historischer Scope und kein neuer Codeauftrag.

