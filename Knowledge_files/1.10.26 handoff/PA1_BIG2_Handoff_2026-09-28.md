# PA1 – Handoff Big2, Stand 28.09.2026

## Startanweisung für den nächsten A-Chat

Du übernimmst die Rolle **A**: Projektverständnis, kritische Methodenprüfung und verständliche Erklärung für den Bearbeiter. **G** arbeitet Corporate-sicher mit dem tatsächlichen Databricks-Stand und kann interne Zeilen und Ausgaben prüfen, die A nicht sehen darf. **C** dokumentiert die Versuchsgeschichte und den Text für die Praxisarbeit. Der Bearbeiter entscheidet über Richtung und Umfang. Frage G bei Abweichungen nach einem technischen Beleg, ohne interne Produktzeilen oder Kundentexte an A zu übertragen.

**Aktuelle Arbeitsentscheidung:** Big2 ist für den Pilot methodisch weit genug entwickelt. Die Variante mit datengetriebener Produktnachbarschaft und produktweiser Aggregation ist der Standard für die nächste Evaluation. Keine neuen Modellmerkmale, Hyperparameter oder Hauptgruppen hinzufügen, bevor die systematische Injektion ausgewertet ist. Diese Entscheidung ist keine Behauptung einer bekannten allgemeinen Fehlerquote.

Dieser Handoff ist eine **Wissensübergabe**, keine neue Codeversion und kein Auftrag, sofort weitere Modelle zu trainieren. Vorhandene Projektdateien zu R1, NMF und dem frühen Reverse-Plan enthalten historisch richtige, teils inzwischen abgelöste Stopppunkte. Bei Widersprüchen gelten die unten datierten Nutzerentscheidungen und die tatsächlich ausgeführten neueren Läufe; ältere Entscheidungen bleiben als Versuchsgeschichte erhalten.

## 1. Projektbild und Verhältnis von Big1, Big2, Big3

Aus einer bereinigten SAP-Stückliste werden prüfbare Anomaliekandidaten gebildet. Die Modelle ergänzen sich: **Big1** lernt, ob eine exakte Komponenten-Materialnummer in einem Produktkontext zu erwarten ist. **Big2** lernt, welche Gesamtmenge und wie viele Positionen derselben Komponente in einem Produktblock plausibel sind. **Big3** soll später fachlich verwandte oder ersetzende Materialnummern und Varianten beurteilen, etwa wenn Big1 eine Spule erwartet, tatsächlich aber eine andere passende Spule verbaut ist. Big3 wird durch einen guten Big2-Befund nicht überflüssig.

Produkte werden nach den ersten sechs Stellen der Produkt-Identnummer in **Hauptgruppen** eingeteilt. Aktuell werden GJL121 und GJL131 getrennt modelliert. `komponente` bezeichnet die exakte Materialnummer; gleicher Kurztext macht zwei Nummern nicht automatisch gleichwertig. Ein **Produktblock** ist ein zusammenhängendes Auftreten eines Produkts im Export. Big2 prüft derzeit vorhandene `(Produkt, Block, Komponente)`-Fälle auf bereinigter **Tiefe 1**. Tiefer liegende eigene Einbaukontexte sind nicht mitbewertet.

Big1 V2 bleibt eingefrorener Anwesenheits-Benchmark. Sein oberster Kandidat mit Auffälligkeit etwa 0,991 wurde vom Fachbereich als historischer Stücklistenfehler bestätigt. Big2 hat einen weiteren bestätigten Mengenfall: Ein gesamter Produktblock war gegenüber vergleichbaren Produkten halbiert. Beide Beispiele sind Nutzennachweise; aus ihnen allein folgt keine allgemeine Precision oder Recall.

## 2. Wie Big2 tatsächlich lernt

Ein Lernfall ist eine vorhandene exakte Komponente in einem Produktblock. Alle Positionszeilen dieser Nummer werden zusammengeführt. Das Lernziel enthält die **Gesamtmenge und die Anzahl der Positionen**. Die geprüfte eigene Menge und Positionszahl werden in den Eingabemerkmalen maskiert; die Mengen und Vorkommen der *anderen* Komponenten des Blocks bleiben als Kontext erhalten. Dazu kommen Komponentenkennung und abgeleitete, allgemeine Merkmale wie Spannung und Nummernbasis. Die Produkt-Folds sind fest: sämtliche Blöcke eines Produkts gehören zum selben der fünf Folds.

Der erste auf alle möglichen Block-Komponenten-Paare ausgedehnte Entwurf hatte 77.400 Fälle und mehr als 100 kategoriale Kontextspalten; zwei MultiClass-Modelle liefen sehr langsam. Die Umstellung auf tatsächlich vorhandene Fälle und numerische, maskierte Kontextwerte brachte ungefähr 14.451 Fälle im ersten GJL121-Pilot und Trainingszeiten im Sekundenbereich. Das war eine Änderung der Analyseeinheit und Eingabedarstellung, keine bloße Erhöhung der Threads.

Frühere reine Mengenlisten enthielten viele gültige Varianten als Kandidaten: dieselbe Komponente kann in unterschiedlichen Produkttypen legitimerweise verschiedene Mengen tragen. Deshalb kamen drei voneinander zu trennende Teile hinzu:

1. **Häufigkeitsbaseline:** Aus den vier Trainingsfolds derselben Hauptgruppe wird `P(Menge | exakte Komponente)` durch blockgewichtetes Zählen mit Laplace-Glättung geschätzt. Sie ist ein Nullpunkt für den Vergleich, nicht der finale Detektor.
2. **Nachbarschaft als fünf Eingaben:** Für jeden Block werden zehn Blöcke mit den ähnlichsten Komponentensets über Jaccard gewählt. Zulässig sind nur Trainingsfolds derselben Hauptgruppe; das eigene Produkt ist ausgeschlossen. Für die Zielkomponente entstehen mittlere Jaccard-Ähnlichkeit, Anteil der Nachbarn mit dieser Komponente sowie Median, Minimum und Maximum ihrer Mengen. Das Modell entscheidet selbst über die Verwendung. Ein zusätzlicher Diagnosewert „Anteil Nachbarn mit derselben Menge“ benutzt die beobachtete Zielmenge und darf **nicht** als Lernmerkmal eingehen. Keine Produkttyp-Präfixe oder produktspezifischen Sollmengen werden kodiert.
3. **Produktaggregation nach der Vorhersage:** Komponenten-Kandidaten eines Produktblocks werden gebündelt. Anzahl der Kandidaten, Summe von `−log(p_ist)` und das Verhältnis `Istmenge/erwartete Menge` zeigen unter anderem ein gemeinsames Halbierungs- oder Verdopplungsmuster. Das ändert kein trainiertes Modell, sondern die Einheit der Rangliste.

Im Entwicklungsvergleich gab es pro Hauptgruppe und Fold je eine Variante mit und ohne die fünf Nachbarschaftsmerkmale, insgesamt **20 trainierte Modelle** laut G. Für den späteren Ladeversuch wurde außerdem ein schlanker Snapshot mit **zehn Modellen der Nachbar-Variante** gespeichert. Diese Zahlen beschreiben unterschiedliche Artefakte und sind kein Widerspruch. Der sichtbare GitHub-Lader verwendet den Zehn-Modell-Snapshot.

`p_ist_menge` ist die vom Modell vergebene Wahrscheinlichkeit für die **beobachtete** Gesamtmenge. `auffaelligkeit_big2` wird bei einer bevorzugten anderen Menge als `1 − p_ist_menge` gebildet. Sie ist eine Ranggröße für einen Widerspruch, **keine kalibrierte Wahrscheinlichkeit, dass fachlich ein Fehler vorliegt**. Neue Mengen oder Komponenten außerhalb des trainierten Zustandsraums brauchen einen expliziten Abdeckungsstatus; ein fehlender Score darf nicht als unauffällig zählen.

## 3. Berichtete Pilot-Ergebnisse und ihr Belegstatus

Die folgende Tabelle stammt aus **Gs Corporate-internem Laufbericht** vom 25.09. A hat die zugehörigen Rohzeilen und Notebookausgaben nicht selbst gesehen. Log-Loss bezieht sich auf prüfbare Fälle innerhalb der jeweiligen Hauptgruppe; die exakten Nenner des jüngsten Laufs sind für die spätere Dokumentation noch mit Gs Export zu sichern.

| Größe | GJL121 | GJL131 |
| --- | ---: | ---: |
| Log-Loss, Häufigkeitsbaseline | 0,442 | 0,580 |
| Log-Loss, Kontextmodell ohne Nachbarn | 0,0222 | 0,0392 |
| Log-Loss, Kontextmodell mit Nachbarn | **0,0177** | **0,0317** |
| Kandidaten, Häufigkeitsbaseline | 2.752 | 3.141 |
| Kandidaten, Modell mit Nachbarn | 48 | 43 |

Nachbarschaft verbessert den berichteten Log-Loss gegenüber demselben Kontextmodell ohne diese fünf Merkmale um rund 20 % in beiden Hauptgruppen, bei unveränderter Bewertbarkeit. **Nur 40 der 91 Kandidaten der Nachbar-Variante** erscheinen auch in der Variante ohne Nachbarn. Die wechselnden Fälle sind ein wichtiges Prüffeld; weniger Kandidaten oder besserer Log-Loss allein beweisen noch keine bessere fachliche Trefferquote.

Die marginale Baseline erzeugt knapp 5.900 Kandidaten in beiden Hauptgruppen zusammen. Eine exakte Komponente kann bei einer Produktfamilie mit Menge 4 und bei einer anderen mit Menge 8 legitim sein. Ihr schlechter Log-Loss und die große Kandidatenzahl stützen die kontextbedingte Modellierung im **gegenwärtigen Pilotbestand**. Das ist keine Aussage, dass Häufigkeitsmethoden grundsätzlich unbrauchbar sind.

Beim **fachlich bestätigten Halbierungsfall** findet Big2 alle neun betroffenen Tiefe-1-Komponenten als Einzelkandidaten. Im älteren gemeinsamen Lauf lag der Fall als einzelne Komponenten etwa auf Zeilenrang 46/47. G berichtet nun für das getrennte Nachbar-Modell `p_ist` von ungefähr 0,025 für eine beispielhafte Zeile statt zuvor 0,088; die Produktaggregation führt den Block als „alle Mengen um Faktor 0,5 verschoben“ auf **Produktrang 1 in GJL131**. Auch ohne Nachbarschaft erreicht der Block im Bericht Produktrang 1: Der unmittelbare Ranghebel ist die Aggregation. Die bloße Mengenhäufigkeitsbaseline erkennt den bestätigten Fall nicht zuverlässig, weil dieselbe Menge 4 anderswo legitim vorkommt. Eine ältere gegenteilige Aussage wurde zurückgenommen.

G berichtet ferner einen plausiblen, **noch nicht fachlich bestätigten** gemischten Block in GJL121 mit elf Kandidaten auf Produktrang 1; weitere einzelne Feder- und Bestückungsfälle bleiben zu prüfen. Aus den Kandidaten dürfen ohne Fachurteil keine bestätigten Fehler werden. Konkrete Corporate-Kennungen und unmaskierte Kurztexte gehören nicht in diesen Handoff.

## 4. Handinjektion und geklärte technische Grenze

G hat in einer **manuell manipulierten bereinigten Stückliste** vier Produktfälle geprüft: zwei Änderungen von Menge 1 auf 2, eine Feder von 4 auf 3 und einen beidseitigen Festschaltstück-Fall von 4 auf 8. Laut Corporate-interner Prüfung wurden **vier von vier** mit der jeweiligen ursprünglichen Menge als Erwartung erkannt. Berichtete Auffälligkeiten: 0,994 bis 0,999; Produktränge 2 bis 6, da davor mehrteilige Produktbefunde liegen. Eine versehentlich statt der Menge geänderte Tiefe wurde zu Recht nicht als Mengenfehler gezählt. G berichtet eine sonst unveränderte Restliste. A hat die intern sichtbaren Zeilen nicht eigenständig verifiziert.

**Klarstellung vom 28.09.:** Bei genau diesem Handversuch wurden Kontext und Nachbarschaft aus der manipulierten Datei berechnet; die CatBoost-Modelle kamen aus dem Snapshot. Das war ein beabsichtigter **produktionsnaher Anwendungstest**: Ein neues zu prüfendes Exportbild liefert seinen aktuellen Kontext. Diese vier Erkennungen bleiben als Beobachtung bestehen. Sie sind kein unverzerrter Schätzer des Recall, zumal Änderungen in einem Produkt als Nachbarinformationen anderer Prüfprodukte auftreten können.

Für die geplante **systematische Injektion** gilt ein anderer, vorher festgelegter Messaufbau: Originalzustände der Trainingsfolds und Nachbar-Referenzen aus dem sauberen Snapshot einfrieren; den veränderten Prüfblock mit seinen neu berechneten aktuellen Eingaben gegen das passende gespeicherte Fold-Modell prüfen. Keine `.fit()`-Aufrufe. Jede Injektion mit Produkt, Block, Komponenten-ID, ursprünglicher und veränderter Menge/Positionszahl, Fehlerart und Fold protokollieren. Eingriffe einzeln oder kontrolliert getrennt auswerten, damit nicht mehrere Manipulationen unbemerkt ihre Nachbarreferenz beeinflussen.

Vorgeschlagener nächster Versuch laut G: etwa 50 Produkte **je Hauptgruppe und Fehlertyp** für Einzelmengenfehler, Blockhalbierung/-verdopplung und zusätzliche gleiche Positionszeile. Berichten: Recall@5/Recall@10 und mittleren Rang jeweils für Komponenten- und Produktbefunde, außerdem Falschalarmrate bei unveränderten Produkten und Fälle ohne Score. Die Zähleinheit und der Nenner müssen je Kennzahl angegeben werden. Das ist **noch nicht durchgeführt**. Künstliche Fehler messen die Erkennung dieser definierten Manipulationen, nicht die unbekannte Recall-Rate aller realen SAP-Fehler.

## 5. Was der aktuell sichtbare Code belegt

Am 28.09. über den GitHub-Connector geprüft: Das Repository heißt **`KyRage187/PA1`** (nicht `kyrage1/pa1`). Die Datei [`CORR_test_gjl131,121 MAIN_Big2_Pilot.ipynb`](https://github.com/KyRage187/PA1/blob/main/CORR_test_gjl131%2C121%20MAIN_Big2_Pilot.ipynb) hatte Blob-SHA `7fa67b79bd1c1ad211616de1031334c6eeff2dbb`, 15 Zellen, keine eingebetteten Ausgaben und keinen `.fit()`-Aufruf. Sie lädt zehn gespeicherte Nachbar-Modelle. Der umfassende 20-Modell-Trainingslauf und die intern gesichteten Ausgaben sind in **dieser** GitHub-Datei nicht enthalten; ihr Vorliegen ist Gs Corporate-Bericht.

Im sichtbaren `CORR_test`-Code zählen die Häufigkeitsbaseline und die Nachbar-Mengen noch aus der aktuell eingelesenen, gegebenenfalls manipulierten Datei (`f_nb`). Die an `prognose_big2` übergebenen Trainingsfälle kommen dort ebenfalls aus `faelle_b2` der aktuellen Datei. Diese Beobachtung widerspricht **nicht** Gs Beschreibung des absichtlich produktionsnahen Handversuchs; sie bezeichnet den Umbau, der für den systematischen Injektionstest nötig ist. Die dortige Vergleichsprüfung gegen gespeichertes `X` wird bei Abweichung nur als Warnung ausgegeben. Ein neuer A-Chat darf nicht behaupten, die eingefrorene systematische Inferenz sei bereits im verlinkten Repo implementiert.

Die sichtbare ältere Datei [`MAIN_Big2_Pilot (2).ipynb`](https://github.com/KyRage187/PA1/blob/main/MAIN_Big2_Pilot%20%282%29.ipynb) umfasst nur einen früheren Ein-Hauptgruppen-Stand; sie ist **nicht** die Quelle des berichteten 20-Modell-Vergleichs. Originalmodelle, Corporate-Daten und interner Snapshot bleiben bei G/Databricks. A prüft nur freigegebene, ausreichend maskierte Ergebnisse.

## 6. Korrigierte frühere Aussagen und offene Grenzen

- **Tiefe:** Die frühere These, ein bestimmter Kandidat sei bloß ein Tiefe-3-Artefakt, war falsch. `clean_bom` hängt Dummy-Kinder korrekt um; die betrachteten Festschaltstücke liegen bereinigt auf Tiefe 1. Dennoch deckt Big2 nur Tiefe 1 ab, nicht automatisch sämtliche möglichen Stücklistenfehler.
- **Baseline beim bestätigten Fall:** Die frühere Vermutung, eine reine Komponentenmengen-Häufigkeit finde diesen Fall schärfer, war falsch. Die legitime Menge in einer anderen Produktfamilie verwischt ihn.
- **Produkttyp-Schlüssel:** Ausführungen aus Produktnamen abzuleiten wurde vorgeschlagen und vom Bearbeiter wegen geringer Übertragbarkeit verworfen. Keine V-/B-Präfix-, Polzahl- oder Sonderproduktregel einführen. Die Nachbarschaft ist datengetrieben.
- **Nachbargrenze:** G berichtet 117 beziehungsweise 84 Fälle ohne Nachbarmenge in den beiden Pilotgruppen. Bei Unikaten fehlen vergleichbare Geschwister. Einen Abdeckungsstatus anzeigen, keine Sicherheit erfinden.
- **Ähnliche Geschwisterprodukte in anderen Folds:** Sie erleichtern die Mengenprognose und können den niedrigen Log-Loss mit erklären. Die Produkt-Folds verhindern dasselbe Produkt im Training; sie sind kein harter Test auf bislang ungesehene Familien.
- **Pool gegen getrennte Modelle:** Gegenwärtig getrennt je Hauptgruppe. Ein früherer Poolvergleich lieferte je Hauptgruppe unterschiedliche Richtungen und wechselnde Kandidaten; keine belastbare generelle Entscheidung für Pooling. Hauptgruppen werden für diesen Pilot nicht stillschweigend gemischt.
- **Mehrfachblöcke:** Die Produktaggregation rechnet bisher je Block; dasselbe Produkt kann mehrfach erscheinen. Ein produktweiser Gesamtbericht steht aus, ohne abweichende Blöcke fälschlich zu einem Aufbau zu verschmelzen.
- **Positionszahl:** Das Lernziel enthält sie, aber die aktuelle Grundlogik prüft eine abweichende Gesamtmenge zuerst. Eine zusätzliche gleiche Positionszeile kann deshalb als „Menge abweichend“ statt als „möglicherweise doppelte Position“ beschrieben werden. Für den geplanten Duplikat-Test Erkennung und korrekten Grund getrennt bewerten.
- **Menge 0 und neue Zustände:** Im früheren Tiefe-1-Pilot wurden keine Null- oder Negativmengen beobachtet; daraus folgt keine fachliche Ausschlussregel. Unbekannte Mengen oder Komponenten erhalten keinen regulären Score. Export-/Cleaning-Verbleib der aus anderen Datenquellen bekannten Nullmengen bleibt eine Datenfrage.
- **Skalierung:** Die Merkmalsidee ist generisch, die gespeicherten Modelle decken nur die beiden trainierten Hauptgruppen und bekannten Produkte/Folds ab. Für neue Hauptgruppen, neue Produkte, aktuelle Live-Exporte und Tiefe > 1 fehlen gesonderte Trainings- und Inferenzentscheidungen.

## 7. Beschlossene Reihenfolge ab diesem Handoff

1. **Systematische Injektion mit eingefrorener Referenz** vorbereiten, mit G auf Original-/Prüftrennung prüfen und ohne Nachtraining durchführen. Resultate pro Fehlerart, Hauptgruppe, Zeile und Produkt mit nachvollziehbaren Nennern berichten. Vorher keine neuen Parameter oder Komponentengruppenexperimente.
2. **Fachbereichskandidaten gezielt labeln:** der gemischte GJL121-Block, ein einzelner Federfall, zwei Produkte mit Verdacht auf zusätzliche Positionen sowie eine auffällige Kontaktbestückung. G kann die echten Kennungen intern zuordnen. Antworten als Labels mit Datum und Begründung sichern, nicht als pauschale Ausnahmefilter.
3. **TRAIN und INFER reproduzierbar trennen:** Modellversion, Featurefolge, Produkt-Fold-Zuordnung, Nachbarbildung, Referenzdaten und Cleaning-Stand in einem Manifest; Test für den unveränderten Originalbestand und für geänderte Mengen. Der sichtbare GitHub-Stand ist hierfür eine Zwischenstufe.
4. **Kleine Ausgabekorrekturen:** Produkt mehrfach vorkommender Blöcke als Produkt zusammenfassbar machen, ohne Blockunterschiede zu verlieren; Grundreihenfolge für Positionszahl prüfen; unbekannte Zustände sichtbar behalten. Big1s ältere Trainings-/Ladezelle und der Widerspruch um `kleinere_gruppe` sind separate Dokumentations-/Codepflege, keine Big2-Hyperparametersuche.
5. **C mit belegtem Stand versorgen:** Ausgangspunkt → langsamer Paarentwurf → ein Mengenmodell → Varianten-False-Positives → Baseline/Nachbarschaft/Aggregation → bestätigter Fall → produktionsnaher Handversuch → systematische Injektion samt Grenzen. Noch nicht durchgeführte Schritte klar als Plan markieren.

Komponentengruppen aus Textähnlichkeit plus wechselseitigem Ausschluss, Elternkontexte unter Tiefe 1, weitere Hauptgruppen und Big3 sind legitime spätere Fragen. Big3 adressiert besonders Varianten und Ersetzungen bei Big1; es wird durch Big2 nicht stillschweigend erledigt.

## Quellen- und Sicherheitsregeln für die Übernahme

- **Fachlich bestätigt:** historischer Big1-Spitzenfall und der konkrete halbierte Big2-Produktblock, jeweils nach Aussage des Bearbeiters/Fachbereichs.
- **G berichtet / Corporate geprüft:** Tabellenwerte, Kandidatenwechsel, vier Handinjektionen, konkrete offene Fachfälle. A hat keine Einsicht in unmaskierte Zeilen oder den laufenden Databricks-Snapshot.
- **A am sichtbaren Code geprüft:** GitHub-Dateipfad, 15 Zellen ohne eingebettete Outputs, Zehn-Modell-Lader, derzeitige Verwendung aktueller Daten für Nachbarn und Trainingsfälle im Injektions-Notebook. Das ist eine Codebeobachtung, kein Gegenbeweis zu Gs internem Laufbericht.
- **Vom Bearbeiter entschieden:** Big2-Standard mit Nachbarn; keine Namenssonderregeln; zunächst methodisch einfrieren und evaluieren; Rollen/Big1/Big3 als getrennte, sich ergänzende Perspektiven.
- **Geplant/offen:** systematische Recall-/Falschalarmmessung, neue Hauptgruppen und Produkte, weitere Fachlabels, Produktionspipeline und spätere Variantenanalyse.

Keine unmaskierten Corporate-Zeilen, Kundennamen, echten Materialnummern oder personenbezogenen Databricks-Pfade in weitere A-/C-Handoffs übernehmen. Die genannten Zahlen immer zusammen mit ihrer Einheit und ihrem Quellenstatus verwenden. Am Ende wird eine Kandidatenliste zur fachlichen Prüfung gezeigt, kein automatisch freigegebener Stücklistenfehler.
