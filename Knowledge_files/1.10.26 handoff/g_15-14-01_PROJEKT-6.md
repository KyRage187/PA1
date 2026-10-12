# 01 — PROJEKT

> **Aktuell 01.10.2026:** Ziel bleibt eine fachlich prüfbare Kandidatenliste für Inkonsistenzen in SAP-Stücklisten. Drei ergänzende Modelle: **Big1 V2** (Anwesenheit exakter Komponenten, eingefroren), **Big2** (Menge/Positionszahl vorhandener Komponenten, Pilot), **Big3** (Anwesenheit unter Kenntnis des Stücklistenaufbaus, seit 01.10. trainiert auf GJL121, vorzeigbar, nicht validiert). Die am 28.09. skizzierte Big3-Idee „Varianten/Ersetzungen“ ist **nicht** das gebaute Big3; sie bleibt eine spätere Aufgabe. Entwicklungsrichtung: ein gemeinsames Notebook mit **einer** Liste für den Fachbereich (GJL121 + GJL131). Details: [G_HANDOUT](G_HANDOUT_Big3_2026-10-01.md), [Big2-Handoff](PA1_BIG2_Handoff_2026-09-28.md).

> **Historisch 28.09.2026:** Big1 und Big2 getrennte Pilotmodelle; Big3 „später möglich, aber nicht trainiert“ — abgelöst am 01.10.

Historische Abschnitte: **09.–16.09.2026**. Aktueller Einstieg: [00](17-00_START_HIER-6-1-%282%29.md). Quellen-/Artefaktverzeichnis: [05](08-05_CODE-5-%281%29.md). NMF-Pilotstand: [08](09-08_NMF_PILOT_STAND_PRUEFPLAN-2-.md).

## Rahmen und Abgabe

| Punkt | Stand |
|---|---|
| Bearbeiter | Studierender Data Science und Künstliche Intelligenz, duale Hochschule, zweite Praxisphase |
| Thema | Entwicklung eines Prototyps zur Erkennung von Dateninkonsistenzen in SAP-Stücklisten mittels Machine Learning |
| Praxispartner / Standort | `<UNTERNEHMEN>` / `<STANDORT>` / `<WERK>` |
| Betriebliche Betreuung | `<BB>`; fachlicher Ansprechpartner `<AP>`; wissenschaftliche Betreuung an der Hochschule |
| Abgabeziel | **16.10.2026** |
| Umfang / Format laut bisherigem Projektstand | 20–30 Seiten Inhalt; LaTeX-Vorlage; Deutsch; digitale und gebundene Abgabe, Abstract und Ablauf-/Reflexionsbericht |
| Kolloquium | Laut bisherigem Stand keines; Erklärbarkeit gegenüber Betreuung/Fachseite bleibt wichtig |
| Offene Formalien | Zitierstil und ggf. Sperrvermerk mit zuständigen Stellen klären |
| Arbeitsweise | Entwicklung und Dokumentation parallel; kein verbindlicher Wochenzeitplan mehr |

## Problem und Ziel

Mögliche Fehler in SAP-Stücklisten sollen gefunden werden, weil falsche Komponenten oder Mengen über die retrograde Entnahme zu falschen Bestandsbuchungen beitragen können. Das betriebliche Motiv wurde nach einem Vortragsfeedback mit „Warum werden andere Sachen gebucht, als in der Tabelle stehen?“ geschärft. Das ist der Hintergrund; der laufende Versuch untersucht zunächst Vergleichbarkeit in der Stückliste.

Das geplante Endprodukt priorisiert mögliche Inkonsistenzen und liefert verständliche Belege. Ein ungewöhnlicher Fall ist nicht automatisch ein Fehler. Fachliche Prüfung bleibt für die Bewertung tatsächlicher Anomalien relevant. Warenbewegungen und geprüfte Fehlerlabels stehen hier nicht zuverlässig als vollständige Trainings-/Validierungsbasis zur Verfügung.

**Der Algorithmus ist das Produkt, nicht die Auswertung genau eines Exports.** Die Stückliste kann später andere Produkte oder nur eine kleinere Gruppe enthalten. Wiederverwendbare Datenaufbereitung und Modelle sind das Ziel. Die im Projekt fachlich bestätigte Trennung nach den ersten sechs Stellen der Produkt-Identnummer bleibt gesetzt; konkrete Präfixwerte und Produktnamen werden nicht als Sonderregellisten in das Rollenmodell eingebaut. Allgemeine Übertragbarkeit ist ein zu prüfendes Ziel, keine schon erreichte Eigenschaft.

## Das gemeinsame Projektverständnis

Die Stückliste enthält Produkte. Diese werden nach den ersten sechs Stellen ihrer Produkt-Identnummer in **Hauptgruppen** getrennt. Hauptgruppen werden niemals miteinander vermischt. Innerhalb einer Hauptgruppe stehen **kleinere Gruppen**, im Gespräch etwa B6, B6S oder VB6A. Kommerzielle Unterschiede in der Bezeichnung können bestehen bleiben, auch wenn wir zwei kleinere Gruppen für die Analyse zusammenführen.

| Aufgabe | Frage | Stand 28.09.2026 |
|---|---|---|
| 1. Komponentenebene / Big3 | Sind unterschiedliche Materialnummern im Kontext Varianten oder Ersatz? | Noch nicht modelliert; Komponentengruppen bis nach Big2-Injektion zurückgestellt |
| 2. Anwesenheit / Big1 | Fehlt eine exakte Komponente oder ist sie zusätzlich? | CatBoost V2 eingefroren; Top-1 fachlich als historischer Fehler bestätigt |
| 3. Menge / Big2 | Passt die Gesamtmenge einer vorhandenen exakten Komponente zum Blockkontext? | Nachbar-Modell mit Produktaggregation; bestätigter Halbierungsfall Produktrang 1; Recall systematisch noch offen |

„Größtmöglich zusammenführen“ bedeutet so weit wie fachlich sinnvoll. Die bloße Anzahl zusammengeführter Gruppen ist kein Qualitätsmaß. Der gemeinsame Name kann schlicht „B6 und B6S“ lauten. Die Produkte werden damit weder für identisch erklärt noch werden ihre unterschiedlichen Komponenten gelöscht.

## Historische Rollenidee (derzeit zurückgestellt)

Eine Rolle beschreibt die Aufgabe einer Komponente in ihrem Aufbau. Der Bearbeiter will, dass ein Verfahren diese Beziehungen aus Daten erkennt, statt für jede Produktgruppe eine eigene Anzahl erlaubter Unterschiede vorzugeben.

Fiktives Beispiel: Eine kleinere Gruppe verwendet Backplate A; eine andere verwendet je Produkt eine von mehreren Backplate-Varianten. Wenn diese Varianten im jeweiligen Kontext dieselbe Aufgabe erfüllen, sollen unterschiedliche Identnummern allein die Gruppierung nicht verhindern. Dagegen können fünf gleichzeitig vorhandene Backplates oder eine Backplate an anderer Stelle eine relevante andere Struktur sein.

Welche Informationen dafür wie stark zählen, ist noch nicht fachlich fertig definiert. Struktur, Position, Menge, Alternativinformationen und Kurztexte werden schrittweise betrachtet. Kein LLM im derzeit gewählten Ansatz. Das Rollenmodell **könnte** das methodische Herz der Arbeit werden; eine funktionierende semantische Rollenerkennung ist noch nicht nachgewiesen.

Der Bearbeiter befürchtet, dass das Modell zu häufig „bitte selbst prüfen“ ausgibt. Diese Sorge ist ein Bewertungskriterium: Das Verfahren muss tatsächlich bei der Auswahl helfen. Eine kürzere Liste ohne Qualitätssicherung wäre kein ausreichender Erfolg. Fachliche Prüfung umfasst später auch Stichproben aus scheinbar eindeutigen Fällen; eine relevante Fehlerklasse darf nicht stillschweigend verschwinden.

## Historische methodische Abzweigung: NMF statt erzwungener Rollenfortsetzung

R1 und R1.1 haben nicht gezeigt, dass Komponentenrollen aus der vorhandenen flachen Struktur zuverlässig genug gelernt werden können. Deshalb wird der Rollenweg nicht künstlich weitergebaut. Der NMF-Pilot setzt direkt auf Aufgabe 2 an: Er beschreibt jedes Produkt durch eine Mischung wiederkehrender Komponentenmustern und vergleicht anschließend kleinere Gruppen über diese Produktprofile.

NMF ersetzt damit nicht nachträglich R1 und lernt keine funktionalen Rollen. Es ist ein separater, unüberwachter Vergleichsansatz. Seine methodische Stärke ist die nichtnegative additive Darstellung; seine aktuelle Grenze ist die noch fehlende Modellwahl, Stabilitätsprüfung und fachliche Interpretation der Matrix H.

## Entwicklungsgeschichte und begrenzte Schlussfolgerungen

Alle alten Zahlen unten sind berichtete Ergebnisse ihrer damaligen Läufe. Details und fehlende Originalartefakte in [05](08-05_CODE-5-%281%29.md); nicht mit der aktuellen R1-Datenbasis vermischen.

| Abschnitt | Versuch / Befund | Was daraus folgt |
|---|---|---|
| Frühe Anomaliesuche | 1.246 von 1.466 auswertbaren Produkten markiert, rund 85 %; nach Weglassen des Seltenheitssignals 18 Fälle | Seltenheit allein erfasst sehr viele legitime Varianten. 85 % ist ein Anteil markierter Produkte, keine mit Labels gemessene Fehlalarmquote. |
| Frühe Gruppierung | Kurztext-/Nummernansätze zeigten 97,7 % gerichtete Übereinstimmung; Strukturversuch mit wenigen Teilen auf Stufe 1 zerfiel in 1.537 Gruppen | Hinweise auf hierarchische Struktur; kein unabhängiger Beweis fachlich richtiger Gruppen. Der konkrete Stufe-1-Versuch war zu spröde. |
| Frühes `rollen.py` | IDF/Kosinus und gemeinsames Auftreten, 11.558 Rollenpaare, 24 Gruppen; darunter eine übermäßig große mit 206 Komponenten | Struktur kann Beziehungen sichtbar machen. Durchketten und Filtern vor Baumbau verfälschten den Versuch. Keine validierte Rollenleistung nachgewiesen. |
| Weitere frühe Versuche | Gesamtformen/Mehrheitsprädiktor und ein scheinbarer Spannungseffekt von ca. 83 %; Permutationstest relativierte letzteren | Nicht jede hohe Kennzahl beantwortet unsere fachliche Frage. Feine Klassen können starke scheinbare Signale erzeugen. |
| Foundation bis 03.09. | Gemeinsamer Cleaner, Baum vor Filtern, gezielte Reparatur verrutschter Stufen | Arbeitsfähige Datenbasis; Übergang von 47.381 zu 47.390 Positionszeilen ist dokumentierte Korrektur. |
| J-Ansatz / Auftrag 04.09. | Hauptgruppen, kleinere Gruppen, Komponentenprofile und Wortheuristik; Mastertabelle und Gruppen-Clustering beauftragt | Fachlich motivierte Ausgangsbasis. Erstwortgleichheit ist keine gelernte Rolle. |
| Nach dem 04.09.-Auftrag | N lieferte Master und Clustering; A prüfte die maskierte Masterdatei | 89 Paarzeilen rechnerisch konsistent; fachliche Gruppengültigkeit blieb offen. „Master/Clustering erst bauen“ ist als nächster Schritt erledigt. |
| Verständigungsrunde dieses Chats | Bearbeiter korrigierte Begriffe, Rollenverständnis und den Umfang der N-Aufträge | Einheitliche Sprache, drei Aufgaben, Schritt für Schritt; unbekannte Rollenkriterien werden iterativ erarbeitet. |
| R0 | N prüfte verfügbare Strukturinformationen und meldete Abflachung, Mehrfachblöcke und Alternativdatenproblem | Erster R1-Versuch kann auf der vorhandenen Basis beginnen; dies ist noch kein Nachweis brauchbarer Rollenerkennung. |
| R1 | Strukturprofile über Eltern-/Nachbarschaftsinformationen, 16.419 Komponentenpaare | Struktur allein zu breit: 5.649 Paare ohne beobachteten Widerspruch; 98,1 % der messbar ähnlichen Paare ohne gemeinsames internes Elternteil. Nur teilweise brauchbar. |
| R1.1 | Lokale Positions-/Slotinformation als genau eine Ergänzung | Gewöhnliche Positionsnummer verschlechtert die Auswahl und verwirft fast alle plausiblen Spulenpaare; `ALT:`-Slots stützen 6/6 bekannte Alternativpaare. Strukturpfad damit vorerst ausgereizt. |
| NMF-Pilot | Binäre Produkt-Komponenten-Matrix für GJL121; Modelle mit 2–15 Mustern; erste Auswertung des 6-Muster-Modells | Erste plausible Gruppenähnlichkeiten sichtbar, obwohl kleinere Gruppen nicht als Trainingseingabe verwendet wurden. Musterzahl, Stabilität und H-Interpretation noch offen. |

Ältere Formulierungen „Rollen funktionieren“, „15 fachlich klar richtig“ oder „19 bestätigte BOM-Fehler“ waren zu stark. Die letzte Aussage wurde durch Ns spätere Analyse des Diskrepanzenblatts relativiert; siehe 03. Historie erhalten, Behauptungsstärke korrigieren.

## Arbeitsstand der früheren R1/NMF-Phase (durch Big1/Big2 abgelöst)

| Bereich | Status |
|---|---|
| Datenbasis / `clean_bom()` | Grundsätzlich freigegeben; Legacy-Recovery gesondert dokumentiert |
| J-Master / bisheriges Clustering | N hat geliefert; maskierte Masterdatei im bisherigen Chat von A geprüft |
| R0 | Abgeschlossen; N-Bericht und begrenzter A-Codeabgleich |
| R1 | Abgeschlossen; Code, maskierte Paar-CSV und N-Bericht vorhanden |
| R1.1 | Abgeschlossen; Position als allgemeines Rollensignal verworfen, `ALT:`-Slot als begrenzter Anker bestätigt |
| Rollenpfad / weitere Erweiterung | Vorerst gestoppt; keine Menge-/Text-/Embedding-Erweiterung freigegeben |
| NMF-Pilot GJL121 | 180 Evaluationsläufe dokumentiert; stabil, aber ohne klaren Knick oder belegten Zusatznutzen; geparkt |
| Zusammenführung kleinerer Gruppen | Noch keine automatische oder fachlich bestätigte Endentscheidung aus NMF |
| Anomalieerkennung und deren Leistungsbewertung | Historische Einstufung; Big1/Big2 wurden inzwischen umgesetzt, systematische Big2-Injektion steht aus |

R0/R1/R1.1 sind **Versuchsschritte des Rollenmodells**. Sie sind nicht die gleichnamigen alten Cleaning-Regeln R1/R2 und auch keine neu nummerierten Kapitel der Praxisarbeit. Der NMF-Pilot ist davon getrennt und gehört zur datengetriebenen Beschreibung und zum Vergleich von Produkten beziehungsweise kleineren Gruppen.

Databricks-Zugang ist laut Bearbeiter vorhanden. Lokales Python bleibt möglich. Eine Plattformauswahl ist derzeit kein Blocker und keine Aufforderung zur Migration.

## Späterer Live-Prozess

Der Fachbereich hat den historischen Export als Entwicklungs- und Testmaterial akzeptiert, gerade wegen bekannter Sonderfälle. Für das endgültige Arbeitsergebnis soll ein aktueller Live-Export den Prozess starten. Ein neuer Export sollte weniger historische Drucklayoutprobleme haben; das tatsächliche Format und die Übertragbarkeit müssen am neuen Bestand geprüft werden.

Fachvalidierung ist jederzeit per E-Mail möglich, wenn die Fälle verständlich aufbereitet sind. Das Weekly ist keine Voraussetzung. Der frühere Kalenderplan mit fest zugewiesenen Methodenwochen ist durch den Wunsch nach schrittweiser, ergebnisabhängiger Arbeit abgelöst; die Abgabe bleibt 16.10.2026.

## C und die parallel entstehende Praxisarbeit

**Ergänzung 01.10.2026 (Big3):** Für C kommen hinzu: Big3-Hypothese (Big1-Fehlbefunde durch Struktur erklärbar), Erkundung an einer nicht aufgelösten geschweißten Baugruppe, der verworfene Excel-Parser-Ansatz und sein Scheitern an der Universalitätsanforderung, Umstellung auf die Cleaner-Ausgabe, finaler Lauf mit 14 Strukturmerkmalen, Wirkung auf die Big1-Top-100 (33/28/18/21), 23 neue Kandidaten, Kontrollfälle, Grenzen (ein Label, Nachbar-Dominanz 36 %, Varianten ungelöst). Korrigierte Aussage: „Big3 nicht trainiert“ (28.09.) → trainiert, nicht gespeichert. Fehlende Abbildung: Fold-Vergleich des finalen Laufs. Quellen und Status: G_HANDOUT Abschnitte 3–5, 9.


C führt ein Versuchsregister und einen Kapitelentwurf. Der aktuelle separate C-Stand ist hier nicht einsehbar; **mit dieser Pflege wurde C nichts zugesandt**. Die folgende Tabelle dokumentiert den damaligen Anschluss bis 16.09.; für C kommen jetzt Big1 V2, Big2-Baseline/Nachbarn/Aggregation, beide Fachbestätigungen, vier Handinjektionen und die noch offene systematische Injektion hinzu. Ausführliche Quellen und Grenzen: Big2-Handoff. Historische Tabelle:

| Ergänzung / Korrektur | Beleg / Heimat | Nutzung für C |
|---|---|---|
| Gemeinsame Sprache und drei Aufgaben | Bearbeiterkorrekturen; AGENTEN_ROLLEN, 01 | Ziel/Scope und methodische Herleitung |
| Master/Clustering tatsächlich geliefert | N-Bericht, As frühere Prüfung; 05/06 | Versuchsregister und Ist-Stand |
| J neutralisiert nach Wortkategorie, bildet keine bewiesenen Eins-zu-eins-Paare | N-Bericht und Master-Audit; 06 | Limitation der Baseline |
| Rollenmodell löst eine andere Aufgabe als Gruppen-Clustering | 06 | Methodendesign |
| R0-Zahlen und Strukturgrenzen | N-R0-Bericht; 03 | Datenbasis, Tabelle Aufbau vor/nach Cleaning |
| R1-Auftrag mit Freigabe und Ausschlüssen | Exakter Wortlaut in 07 | Reproduzierbares Versuchsregister; Ergebnis bleibt leer |
| Gefahr einer großen manuellen Prüfliste | Nutzerbedenken; 00/06/07 | Bewertungskriterien und Limitationen |
| Korrigierte Übertreibungen zu Wahrscheinlichkeit, Unabhängigkeit und Automatik | 06 | Wissenschaftliche Begriffsschärfe |
| R1: Strukturähnlichkeit misst im flachen Bestand häufig gemeinsames Produktvorkommen statt Funktion | N-Bericht, geprüfter Code/CSV; 05/06/07 | Negatives beziehungsweise teilweise brauchbares Versuchsergebnis |
| R1.1: Positionsnummer ungeeignet, `ALT:`-Slot begrenzt nützlich | N-Bericht, geprüfter Code/CSV; 05/06/07 | Begründete Methodenverwerfung und positive Teilinformation |
| Wechsel zu NMF als separatem Gruppenvergleich | Bearbeiter/Notebooklauf; 06/08 | Methodische Abzweigung, nicht als lineare Erfolgsgeschichte glätten |
| NMF-Snapshot GJL121 und plausible Paarbefunde | `Wahres_ML.csv`; 03/08 | Vorläufige Ergebnistabelle, ausdrücklich ohne optimale Musterzahl oder Fachbeweis |

Bereits als Textbasis nutzbar: Problem, betrieblicher Zusammenhang, Datenherkunft, Cleaner und seine Historie, J-Baseline, Rollenforschungsfrage. Fachliche Rollenrichtigkeit, Generalisierung und Fehlerkennzahlen bleiben vorläufig bzw. fehlen.

Geeignete noch fehlende Darstellungen: vereinfachtes Beispiel desselben Aufbaus vor/nach Cleaning, R1-Ähnlichkeitsverteilung mit klarer Zähleinheit, wenige begründete positive und widersprüchliche Beispiele sowie eine Übersicht verfügbarer Fachurteile. Keine solchen Ergebnisse erfinden, bevor N liefert.

Vorläufige Kapitel: Einleitung; SAP-/Stücklistengrundlagen; Projekt/Daten; Methodik; Umsetzung; Ergebnisse/Bewertung; Fazit/Ausblick. Die spätere Auswahl nach Relevanz darf wesentliche Fehlschläge oder Gegenbefunde nicht auslassen.

## Fachliche Ansprechpartner nach Zweck

| Fachseite | Benötigter Beitrag |
|---|---|
| Montage / Konstruktion | Aufgabe, Einbaukontext und Vergleichbarkeit konkreter Komponenten |
| Stammdatenpflege | Pflegekonventionen, Varianten und mögliche Datenfehler |
| Fertigungssteuerung | Retrograde Entnahme, Versionen und Korrekturabläufe |
| Controlling / Inventur | Größenordnung der betrieblichen Auswirkungen |
| Betriebliche / wissenschaftliche Betreuung | Scope, Arbeit und fachliche Prioritäten |

Keine automatischen E-Mails oder Nachrichten sind durch den Dokumentationsauftrag beauftragt.