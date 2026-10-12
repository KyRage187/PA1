\# 03 — Datensatz: Gesamtbestand, Cleaner-Strukturfelder und Big3-Stand



> \*\*Aktualisierung 01.10.2026 — geprüfter Datenstand (Big3 / GJL121):\*\* 47.390 bereinigte Positionszeilen im Gesamtbestand, davon \*\*43.251 Zeilen hochgezogen\*\* (`stufe > tiefe`, 91,3 %). 20.066 Positionszeilen in GJL121, 774 Produktblöcke, 103 Komponenten in der Matrix. 5.319 Zeilen mit echter Unterbaugruppe (`tiefe > 1`), 7.160 Zeilen mit Alternativgruppe (`alt\_gruppe`). Big3-Strukturmodell trainiert rein auf `sauber`.



\## Neuer Stand: Cleaner-Strukturfelder und Big3 (01.10.2026)



| Kennzahl / Feld | Wert / Ausprägung | Bedeutung / Herkunft |

|---|---|---|

| \*\*Gesamtbestand bereinigt\*\* | 47.390 Zeilen | Aus `clean\_bom()`, Cmd 3 (Assert im Notebook) |

| \*\*GJL121 Bestand bereinigt\*\* | 20.066 Zeilen | Aus `sauber`, Cmd 21 |

| \*\*Produktblöcke GJL121\*\* | 774 Blöcke | `block\_sets` |

| \*\*Komponenten GJL121\*\* | 103 | Spalten in `X\_pilot` |

| \*\*Hochgezogene Zeilen (Gesamt)\*\* | \*\*43.251\*\* (91,3 %) | Zeilen mit `stufe > tiefe` (Phantom-Auflösung durch Cleaner) |

| \*\*Zeilen `tiefe > 1` (GJL121)\*\* | 5.319 Zeilen | Echte Unterbaugruppen nach Cleaning (z.B. geschweißtes Joch) |

| \*\*Zeilen mit `alt\_gruppe` (GJL121)\*\* | 7.160 Zeilen | Vorhandene Alternativpositions-Kennzeichnung |

| \*\*`sobsl\_werte` in `sauber`\*\* | 10 (8.445) / 30 (1.258) / Rest leer (37.687) | Keine Sonderbeschaffungsschlüssel 50/35 in `sauber` |



\### Vollständige Spaltenliste in `sauber` (Stand 01.10.2026)

`\_roh`, `\_weg\_kopie`, `alt\_gruppe`, `alt\_rang`, `alt\_wahrsch`, `block`, `eltern`, `eltern\_orig`, `eltern\_teil`, `erzeugnis`, `erzeugnis\_txt`, `komponente`, `komponente\_txt`, `me`, `menge`, `node`, `posnr`, `postp`, `retrograd`, `schuettgut`, `slot`, `sobsl\_werte`, `stufe`, `teil\_id`, `tiefe`, `zeile\_excel`.



\### Bedeutung der Strukturfelder für Big3

\- `stufe`: Ursprüngliche SAP-Stufe vor dem Cleaning (z. B. Joch 5, Gehäuse 1).

\- `tiefe`: Hierarchie-Ebene nach dem Hochziehen (1 wenn Phantom entfernt, 2 wenn unter echter Baugruppe).

\- `stufe > tiefe`: Kennzeichnet Zeilen, die durch Auflösung von Phantom-Baugruppen hochgezogen wurden (Basis für `b3\_x\_traeger\_phantom\_anteil`).

\- `eltern`: Knoten der übergeordneten echten Baugruppe oder `@ERZ <Produkt>#<Block>`.

\- `slot`: `<eltern\_orig> | POS:<posnr>` oder `| ALT:<alt\_gruppe>`.

\- `alt\_wahrsch`: Einsatzwahrscheinlichkeit (EWAHR). Bei geprüften Alternativen meist 100, Feature Importance in Big3 sehr gering (0,26–0,94).



\---



\## Historischer Stand 28.09.2026 (Big2-Pilotumfang)



\## Big2-Pilotumfang und Zähleinheiten (G-Bericht, 25.09.2026)



Der Gesamtbestand oben ist nicht der Big2-Trainingsumfang. Im ersten GJL121-Pilot waren es 772 Produkte, 774 Produktblöcke, 14.747 Tiefe-1-Positionszeilen und \*\*14.451 vorhandene `(Produkt, Block, Komponente)`-Fälle\*\*. Der verworfene vollständige 77.400-Paarraum zählte auch nicht gelistete Komponenten und trainierte einen anderen, langsamen Versuch. GJL131 kommt später als \*\*eigene\*\* Hauptgruppe dazu. Die aktuellen exakten Nenner je Hauptgruppe, Fold und Bewertungsstatus müssen mit Gs neuer Ergebnistabelle gesichert werden; nicht die 14.451 des Ein-Gruppen-Versuchs auf beide Gruppen hochrechnen.



Für jede vorhandene Komponente werden Positionszeilen innerhalb eines Blocks zu Gesamtmenge und Positionszahl zusammengefasst. Die bereinigte Tiefe 1 definiert die gegenwärtige Prüfung; `stufe` ist die Exportstufe und nicht ersatzweise die bereinigte `tiefe`. Andere Komponenten des Blocks dienen als verdeckungsfreier Kontext. Sämtliche Blöcke derselben Produktnummer liegen im selben Fold, bleiben bei der Mengenanalyse aber getrennte Strukturen. G berichtet in diesem Tiefe-1-Umfang keine Null- oder Negativmengen; Ursache/Verbleib früher erwähnter Nullmengen bleibt offen. 117 beziehungsweise 84 Fälle in den zwei Pilotgruppen haben laut G keine Nachbarmenge; für unbekannte Werte ist ein Abdeckungsstatus erforderlich.



Quelle: Gs Corporate-Laufbericht und vom Nutzer übermittelte frühere Notebooktabellen. A hat keine Rohdaten-Nachzählung. Die Ergebnistabelle, Rangliste und Grenze liegen im \[Big2-Handoff](PA1\_BIG2\_Handoff\_2026-09-28.md).



\## Historischer R1-/NMF-Datenstand



\## NMF-Pilotmenge GJL121



Der NMF-Pilot verwendet nicht alle 1.692 Produkte gleichzeitig, sondern zunächst ausschließlich die größte Hauptgruppe GJL121.



| Größe | Wert | Status / Bedeutung |

|---|---:|---|

| Produkte in GJL121 | 772 | Zeilen der binären Eingabematrix X |

| verschiedene Komponenten in GJL121 | 103 | Spalten der Eingabematrix X |

| Einträge von X | 772 × 103 | 1 = Komponente im Produkt vorhanden, 0 = nicht vorhanden; Mengen und Mehrfachvorkommen gehen nicht ein |

| kleinere Gruppen in der Pilotansicht | 13 | Alle beobachteten Gruppen einschließlich sehr kleiner Gruppen und `BASISTEILE` |

| Summe `produkte\_N` in `Wahres\_ML.csv` | 772 | A geprüft; vollständige Abdeckung der Pilotprodukte |

| mögliche Paare kleinerer Gruppen | 78 | \\(\\binom{13}{2}\\); Kombinatorik, keine Wahrscheinlichkeit |



Die 13 kleineren Gruppen besitzen 1 bis 310 Produkte. Sehr kleine Gruppen bleiben sichtbar, liefern aber instabile Mittelwerte: B6SE hat 1 Produkt, MC09.01/MC09.10 und VB6S jeweils 2, VB6SA 3. B6 besitzt 255 und B6S 8 Produkte. Jede Zahl bezieht sich auf Produkte, nicht Positionszeilen.



`Wahres\_ML.csv` enthält sechs durchschnittliche Musteranteile je kleinerer Gruppe. Die sechs Werte jeder Zeile summieren sich wegen Rundung zwischen 0,9999 und 1,0001. Die Datei enthält \*\*nicht\*\* die ursprüngliche Produkt-Komponenten-Matrix X, nicht die rohe W-Matrix und nicht H.



\## Geprüfte R1/R1.1-Ergebnisdateien



| Datei | Von A geprüft | Zähleinheit / Befund |

|---|---|---|

| `01\_komponentenpaare.csv` | 16.419 × 26 | Komponentenpaare innerhalb von vier Hauptgruppen; Klassen 2.120 K0, 3.580 K1, 5.070 K2, 5.649 K3 |

| `01\_paare\_vergleich.csv` | 16.419 × 40 | dieselbe Paargrundgesamtheit; nach Position 5.481 K2b und 168 K3, zusätzlich unverändert 5.070 K2, 3.580 K1, 2.120 K0 |

| `02\_pruefbelege\_lang.csv` | 10.113 × 17 | lange Merkmalsbelege für 299 ausgewählte Paare laut N-Bericht |



Die geprüfte R1.1-Paar-CSV trägt Codeversion `R1.1-v1` und Lauf-ID `20260908-080910`. Der begleitende Bericht nennt dagegen `20260908-080501`; diese Differenz ist offen und darf nicht stillschweigend vereinheitlicht werden.



\## Was genau gezählt wird



| Größe | Zahl laut N | Bedeutung |

|---|---:|---|

| Bereinigte Positionszeilen | 47.390 | Vorkommen von Komponenten; nicht 47.390 verschiedene Identnummern |

| Produkte | 1.692 | Unterschiedliche Produkt-Identnummern |

| Produktblöcke | 1.822 | Zusammenhängende Produktabschnitte des Exports |

| Produkte mit zwei Blöcken | 130 | Mehrere Blöcke sind keine zusätzlichen Produkte |

| Direkte Kinder der Produktwurzel | 33.545 | Positionszeilen ohne verbliebene interne Baugruppe darüber |

| Positionszeilen mit internem Elternteil | 13.845 | Laut N vollständig über Knoten-ID auflösbar; 0 verwaiste Verweise |

| Gruppen direkter Kinder | 4.870 | Gleicher Elternknoten innerhalb desselben Blocks; höchstens 28 Kinder, 21 Gruppen mit nur einem Kind |

| Slots | 46.428 | Technische Positions-/Alternativzuordnungen; keine Rollenanzahl |

| Zeilen mit Alternativinformation | 16.129 | 34 % der Positionszeilen; `alt\_wahrsch` überall 100 |

| Übrige Zeilen | 31.261 | `alt\_wahrsch` überall 0 |



N meldet `stufe` von 1 bis 10 und bereinigte `tiefe` von 1 bis 5; beide unterscheiden sich bei 91,3 % der Zeilen. 5.926 ursprüngliche Stufensprünge sind kein Nachweis räumlicher Positionen. `eltern` und `eltern\_orig` unterscheiden sich bei 35.697 Zeilen.



\## Felder und ihre Grenzen



Produkt- und Komponenten-Identnummer sowie zugehöriger Kurztext sind laut N vollständig vorhanden. Kleinere Gruppen wurden über das erste Wort vor dem ersten Bindestrich abgeleitet. Vollständige Ableitung bedeutet noch keine fachlich bestätigte Zuordnung jeder Bezeichnung.



`eltern` verweist auf einen Knoten, dessen Komponenten-Identnummer aufgelöst werden muss. `eltern\_teil` ist im beigefügten Code eine Textkennung, nicht automatisch diese Identnummer. `eltern\_orig` kann auf eine inzwischen entfernte Position zeigen. Vorhandene Slots verwenden ursprüngliche Zuordnungen, um Alternativen beieinander zu halten; ihre Auswertung ist deshalb von einer Verwendung als Rollenmerkmal zu unterscheiden.



N berichtet 9 `teil\_id`-Werte mit mehreren Schreibweisen sowie 4 Komponenten mit auseinanderfallenden Kategorien durch unterschiedliche Umlautbehandlung in Auswertungen. Diese Befunde sind nicht durch erneute Ausführung der aktuellen Module bei A bestätigt.



\## Mehrfachblöcke korrekt behandeln



Ein von N beschriebenes Produkt hat zweimal 18 Positionszeilen, zusammen aber 19 unterschiedliche Komponenten. Die Blöcke sind daher nicht automatisch identische Kopien. Für R1 bleiben die Bäume getrennt. Beim Zählen derselben Information darf dieselbe Produkt-Identnummer trotzdem höchstens einmal beitragen. Nicht willkürlich einen Block löschen und keine gemeinsame Verwendung über Blockgrenzen erfinden.



\## Ältere Zahlen und Ergebnisdateien einordnen



\- 47.381 Zeilen gehören zum älteren Lauf ohne die vorgeschaltete Reparatur von 144 Legacy-Zeilen; 47.390 zum berichteten korrigierten Lauf mit 9 zusätzlichen Positionen. Das ist keine allgemeine Reparaturregel für zukünftige Exporte.

\- Historischer Rohumfang: 117.264 Zeilen, davon 113.677 eigentliche Datenzeilen nach Abzug von 1.821 Kopf- und 1.766 Leerzeilen.

\- J und Gruppen-Clustering: 23 Kombinationen aus Hauptgruppe und kleinerer Gruppe; 89 Paarvergleiche innerhalb von drei Hauptgruppen. N berichtet 1.656 berücksichtigte Produkte und 36 außerhalb dieses Vergleichs. Die Erklärung für die 36 ist keine A-Neuberechnung am Rohdatensatz.

\- A prüfte `01\_master\_typpaare.csv`: 89 Paarzeilen, 9 Entscheidungen ZUSAMMEN, 80 GETRENNT, 15 resultierende Gruppen; alle 89 Expertenfelder noch OFFEN. Die vom Nutzer bestätigte Zusammengehörigkeit B6/B6S ist davon getrennt zu dokumentieren.

\- Die alte Formulierung „19 bestätigte BOM-Fehler / einzige Wahrheitsquelle“ ist zurückgenommen. N unterscheidet später 17 Fälle: sechs fehlende Referenzen, einen Variantenfall und zehn vermutete Zuordnungsfälle. 19 Zeilen und 17 Fälle sind nicht abschließend reconciliert; daraus keine Fehlerquote berechnen.

\- Die vermeintlich maskierte Masterdatei enthält weiterhin lesbare Texte. Sie nicht als vollständig anonymisiert weitergeben. Nutzerbeispiele B6/B6S/VB6A können zur Erklärung dienen; keine zusätzlichen sensiblen Originaltexte in die Übergabe aufnehmen.



Die folgende historische Bestandsaufnahme bewahrt unter anderem Herkunft und die 36-Felder-Tabelle. Ihre alten Statuswörter, Wahrheitsbehauptungen und nächsten Aufträge gelten nur für den damaligen Stand und werden durch die obigen Korrekturen ersetzt.



<details>

<summary>Historischer Wissensstand bis 04.09.2026 (zum Nachschlagen)</summary>



\## 03 — DER DATENSATZ



> Unternehmensneutral. Keine Datei-, Blatt-, Produkt-, Material- oder Kundenbezeichnungen.

> Aggregierte Kennzahlen sind enthalten, weil sie die methodischen Schlüsse tragen.



\---



\### Herkunft und Rahmen



\- \*\*Ein Stücklisten-Export als Excel-Arbeitsmappe\*\*, ca. 14 MB, \*\*Exportstand 2019\*\*

\- Ursprünglich als \*\*Excel-Binärformat\*\* (`.xlsb`) erhalten → das Upload-Format wurde nicht akzeptiert → in `.xlsx` umgewandelt

\- \*\*`.xlsx` ist das richtige Arbeitsformat.\*\* Lädt in \~0,2 s mit `openpyxl(read\_only=True)` — Größe ist nie das Problem

\- \*\*⚠️ Die Datei stammt aus dem OPERATIVEN System\*\*, nicht aus einem Testsystem

&#x20; → \*\*Bei Widersprüchen zwischen Export und Testsystem gilt der Export\*\*

&#x20; → und: es sind \*\*Produktivdaten\*\*, entsprechend behandeln



\### Erzeugt mit



Einem \*\*kundeneigenen Report zur Stücklistenauflösung für Fertigungsstücklisten\*\* (`<Z-REPORT>`).



\- Gewachsen seit \*\*1999\*\*, letzte Änderungen \*\*Ende 2025\*\*

\- Kommentar im Quellcode: \*„wird in der gesamten Montage genutzt"\* → \*\*„Mont" in den Notizen des Bearbeiters = Montage\*\*

\- \*\*Im Dialog auf 20 Stücklisten begrenzt\*\*, darüber ist Hintergrundeinplanung nötig → daher die Notiz „im Hintergrund laufen lassen"



\*\*Wesentliche Selektionsparameter:\*\*

| Parameter | Default |

|---|---|

| Materialien | – |

| Werk | vorbelegt mit dem Produktionsstandort |

| Auflösungstiefe | \*\*99\*\* = alle Ebenen |

| \*\*Stichtag\*\* | Systemdatum |

| Verwendung | \*\*3 = universal\*\* |

| Alternative | \*\*01\*\* |

| Fertigungsversion | 1–99 |

| Einsatzmenge | 1 |



\*\*Die Selektionsvariante\*\* wurde vom Bearbeiter zunächst mit einem Tippfehler notiert (ein Buchstabe falsch). Der korrekte Name steht im System.



\---



\### Struktur der Arbeitsmappe



Sieben Blätter:



| Blatt | Größe | Status |

|---|---|---|

| \*\*Arbeitsblatt\*\* (die aufgelöste Stückliste) | 117.265 × 27 | \*\*hier passiert alles\*\* |

| \*\*Blatt mit geprüften Abweichungsfällen\*\* | 19 × 8 | \*\*wichtig, siehe unten\*\* |

| Selektionsblatt | 9.748 × 14 | \*\*nie ausgewertet\*\* |

| Erzeugnisliste | 2.303 × 27 | \*\*nie ausgewertet\*\* |

| Änderungsblatt 2017–2019 | 636 × 8 | \*\*nie ausgewertet\*\* |

| zwei Restblätter | klein | ein Beispielgerät + Reste |



> ⚠️ \*\*Der Name des Arbeitsblatts endet auf ein Leerzeichen.\*\* Wer es im Code entfernt, bekommt `KeyError`. Nicht „korrigieren".



\---



\### Kennzahlen des Arbeitsblatts



```

117.264  Datenzeilen

113.677  echte Datenzeilen

&#x20; 1.821  WIEDERHOLTE KOPFZEILEN mitten in den Daten

&#x20; 1.766  Leerzeilen

&#x20; 1.692  Erzeugnisse

&#x20; \~1.400 verschiedene Komponenten

&#x20; 2.530  Eltern-Kind-Einheiten rekonstruierbar (Ø 4,2 Kinder, min 1, max 14)

```



\#### Verteilungen (vor Bereinigung)



\*\*Auflösungsstufe:\*\* Stufe 1 → \~12.000 Zeilen, Stufen 2–5 jeweils \*\*15.000–23.000\*\*, Stufen 6–8 abnehmend bis \~4.800.

→ \*\*Stufe 1 ist nur \~11 % der Daten.\*\* Wer nur Stufe 1 nimmt, ignoriert 89 %.



\*\*Mengeneinheiten:\*\* `ST` \~86.000 · Liter \~15.000 · `KG` \~4.500 · Gramm \~4.200 · Meter \~2.400 · Millimeter \~1.800 · Rest vernachlässigbar

→ Neben Stückgut gibt es relevante Mengen an Flüssigkeiten und Gewichtsmaterial (Verbrauchsstoffe).



\*\*Positionstypen:\*\* `L` \~105.000 · `X` \~9.000 — \*\*nur diese zwei\*\*



\*\*Mengen:\*\* `1` \~57.000 · `2` \~17.000 · `0,001` \~12.000 · `4` \~5.900 · \*\*`0` \~5.300\*\* · `8` \~2.100



\*\*Gültig-ab-Datum:\*\* Stände von \*\*2001 bis 2019\*\* parallel im Datensatz. Häufigste Einzelstände umfassen jeweils 10.000–32.000 Zeilen.



\*\*Klassifizierungsmerkmal zur Bauteilart:\*\* nur \*\*19 % gefüllt\*\*. Enthält generische Bauteilkategorien (Gehäuseteile, Verpackung, Kontaktteile, Isolierteile, Hilfs-/Betriebsmittel …). \*\*Wäre der billigste Weg zur Substitutionserkennung — ist aber zu lückenhaft.\*\*



\---



\### Produktinhalt (bewusst unspezifisch)



\- Der Export enthält \*\*eine Produktdomäne\*\*, nicht das gesamte Portfolio. Andere Produktbereiche sind \*\*nicht\*\* enthalten.

\- Mehrere verwandte Baureihen mit jeweils \*\*20–30 Varianten\*\*

\- \*\*Die Varianten unterscheiden sich fast durchgängig über eine elektrische Kenngröße\*\* (Spannung) → \*\*der Variantenträger ist eine bestimmte Baugruppe\*\*

\- \*\*Kundenspezifische Varianten\*\* kommen vor und sind am Kurztext erkennbar (Kundenname oder Kundenteilenummer im Text). Sie haben \*\*planmäßig\*\* Sonderteile — Sondergehäuse, Sonderverpackung, Kundenetikett.

&#x20; → \*\*Das ist kein Fehler.\*\* Und es war die Ursache der 85-%-Fehlalarmquote von Ansatz 1.

\- Beobachtet wurde außerdem ein \*\*Tippfehler in einem Materialkurztext\*\* (fehlender Anfangsbuchstabe). Belegt: Kurztexte sind \*\*nicht fehlerfrei gepflegt\*\* → exakter Textvergleich ist zu spröde, es braucht Toleranz.



\---



\### Das Blatt mit den geprüften Abweichungsfällen — die einzige Wahrheitsquelle



19 Zeilen, offenbar \*\*von Hand geprüft\*\*. Struktur: Vergleich zwischen \*\*„welche Komponente sagt die Referenzquelle"\*\* und \*\*„welche Komponente steht in der Stückliste"\*\*.



\*\*Das Fehlermuster (generisch beschrieben):\*\*

1\. Die elektrische Kenngröße im \*\*Gerätenamen\*\* passt nicht zur Kenngröße in der \*\*Komponentenbezeichnung\*\*

2\. \*\*Strom-Art verwechselt\*\* (Wechsel- statt Gleichspannung) bei ansonsten gleicher Kenngröße

3\. Die erwartete Komponente ist in der Stückliste \*\*gar nicht gefunden\*\*



\#### Warum das Blatt wichtig ist

\- \*\*Die einzigen bestätigten Fehlerfälle\*\*, die existieren → Validierungsmenge

\- \*\*Alle vom selben Typ\*\* → deckt nicht alle Fehlerarten ab

\- Die Fälle liegen auf \*\*Stufe 2/3\*\*, nicht auf Stufe 1 → \*\*eine Stufe-1-Analyse kann sie prinzipiell nicht finden\*\*

\- Sie belegen den Punkt des Bearbeiters: die Rolle ist besetzt, \*\*aber falsch\*\* → dritte Prüfung „passen die Besetzungen zueinander"

\- \*\*Das Rollen-Skript hat die betroffene Rolle von selbst gefunden\*\*, ohne dass es jemand gesagt hat



\*\*Wer die 19 gefunden hat und mit welcher Methode — OFFEN.\*\*



\---



\### ⚠️ DAS FORMATPROBLEM — Ursache geklärt



\#### Es liegt NICHT an der Formatumwandlung



Im Selektionsbild des Reports gibt es eine Option für einen \*\*tabulatorgetrennten Export für Tabellenkalkulationen\*\*. Diese Option kam erst mit einer Änderung im \*\*April 2024\*\* in den Report (\*„correction list layout and new ALV layout"\*).



> ### Der Export ist von 2019 — also aus der Zeit VOR dieser Korrektur. Der alte Listendruck ist die Ursache. Ein neuer Export liefert saubere Spalten.



\#### Die Symptome

| Symptom | Detail |

|---|---|

| \*\*Kopfzeile steht 1.821× als Datenzeile\*\* | gedruckte Liste mit Seitenumbrüchen, Spaltennamen pro „Seite" wiederholt |

| \*\*Umlaute zerschossen (Mojibake)\*\* | UTF-8-Bytes als GBK interpretiert. Reparatur in Python: `text.encode('gbk').decode('utf-8')` mit Fallback |

| \*\*Kopfzeilennamen um ein Zeichen verschoben\*\* | Namen laufen in die Nachbarzelle über |

| \*\*Mehrere SAP-Felder in einer Excel-Zelle\*\* | rechts im Blatt; erkennbar an Werten wie `"X    1"` oder zwei Codes mit Leerzeichen getrennt |



\#### Screenshot-Erkenntnisse aus dem Selektionsbild (erklären die Daten)

| Einstellung | Folge im Datensatz |

|---|---|

| Hilfs- und Textpositionen \*\*ausblenden\*\* war aktiv | \*\*deshalb nur die Positionstypen `L` und `X`\*\* |

| Alternativpositionen ausblenden war \*\*nicht\*\* aktiv | \*\*Alternativpositionen sind drin\*\* → Ursache der \~5.300 Nullmengen |

| „Auflösen nach Dummystruktur" war aktiv | Dummys werden durchgerechnet |

| Informationspositionen verbergen war \*\*nicht\*\* aktiv | die \~9.000 `X`-Positionen sind drin |

| Selektion über \*\*Fertigungsversion\*\* | nicht über Stücklistenverwendung |

| Einsatzmenge = 1 | Mengen beziehen sich auf ein Stück des Kopfmaterials |



\---



\### DIE AUTORITATIVE SPALTENLISTE — 36 Felder



Quelle: die \*\*Ausgabestruktur des Reports\*\* im SAP-Dictionary plus die Datenelement-Bezeichnungen.

\*\*Das ist die Referenz — nicht eine Rekonstruktion aus dem zerschossenen Export.\*\*



| # | Datenelement | Offizielle Bezeichnung | im Export? |

|---|---|---|---|

| 1 | `MATNR` | Materialnummer \*(Erzeugnis)\* | ✅ |

| 2 | `MAKTX` | Materialkurztext \*(Erzeugnis)\* | ✅ |

| 3 | `IDNRK` | Stücklistenkomponente | ✅ |

| 4 | `OJTXP` | Objektkurztext (Position) \*(Komponente)\* | ✅ |

| 5 | `VERID` | \*\*Fertigungsversion\*\* | ❌ \*\*fehlt\*\* |

| 6 | `DGLVL` | Auflösungsstufe (aufbereitet) | ✅ |

| 7 | `CS\_E\_MNGKO` | errechnete Komponentenmenge in Komponenten-ME | ⚠️ eine der drei |

| 8 | `MEINS` | Basismengeneinheit | ✅ |

| 9 | `POSTP` | Positionstyp (Stückliste) | ✅ |

| 10 | `DTVON` | Datum gültig ab/von | ✅ |

| 11 | `SPOSN` | Nummer der Stücklistenposition | ✅ |

| 12 | `SCHGT` | Kennzeichen: Schüttgut \*(Position)\* | ⚠️ |

| 13 | `CS\_DISPO` | \*\*Auflösungssteuerung\*\* | ⚠️ verschmolzen |

| 14 | `CS\_SOBSL` | \*\*Sonderbeschaffungsart der Stücklistenposition\*\* | ⚠️ verschmolzen |

| 15 | `CS\_ALPGR` | Alternativposition: Gruppierung | ✅ |

| 16 | `CS\_ALPRF` | Alternativposition: Rangfolge | ✅ |

| 17 | `CS\_ALPST` | Alternativposition: Strategie | ✅ |

| 18 | `EWAHR` | Alternativposition: \*\*Einsatzwahrscheinlichkeit in %\*\* | ✅ |

| 19 | `SOBSL` | \*\*Sonderbeschaffungsart\*\* \*(Materialstamm)\* | ✅ |

| 20 | `SCHGT` | Kennzeichen: Schüttgut \*(Materialstamm)\* | ✅ |

| 21 | `RGEKM` | \*\*Kennzeichen: retrograde Entnahme\*\* | ✅ |

| 22 | `SANKO` | Konstruktionsrelevanz Position | ✅ |

| 23 | `SANFE` | Fertigungsrelevanz Position | ✅ |

| 24 | `SANKA` | Kalkulationsrelevanz Position | ⚠️ mit 25 verschmolzen |

| 25 | kundeneigenes Feld | Alternativkomponente | ⚠️ mit 24 verschmolzen |

| 26 | `ATWRT` | Merkmalwert \*(Lizenznehmer)\* | ✅ |

| 27 | `ATWRT` | Merkmalwert \*(\*\*Bauteilart\*\*)\* | ✅ |

| 28 | `ATWRT` | Merkmalwert \*(Auftragsdruck)\* | ✅ |

| 29 | `HRKFT` | Herkunftsgruppe | ⚠️ mit 30 verschmolzen |

| 30 | `BKLAS` | Bewertungsklasse | ⚠️ mit 29 verschmolzen |

| 31 | `BESKZ` | Beschaffungsart \*(Eigen/Fremd/beides)\* | ✅ |

| 32 | `VERKZ` | Verarb.kennzeichen Sonderbestandspflege | ⚠️ mit 33 verschmolzen |

| 33 | `ATWRT` | Merkmalwert \*(Klassifizierung)\* | ⚠️ mit 32 verschmolzen |

| 34 | `STLST` | Stücklistenstatus | ✅ |

| 35 | `CS\_E\_MNGKO` | \*\*Komponentenmenge gewichtet mit Einsatzwahrscheinlichkeit\*\* | ❌ \*\*fehlt\*\* |

| 36 | `KMPMG` | Komponentenmenge | ⚠️ eine der drei |



\#### Drei Konsequenzen



\*\*1. Die linke Hälfte (Felder 1–11) ist eindeutig zuordenbar.\*\* Damit kann sofort gearbeitet werden.



\*\*2. Die rechte Hälfte nicht.\*\* Dort sind Felder verschmolzen — und \*\*genau dort liegen die beiden Sonderbeschaffungs-Felder\*\* (14 und 19), also die Grundlage der Dummy-Regel R5/R6. Im Export gibt es \*\*zwei Kandidatenspalten\*\* mit plausiblen Werten und unterschiedlichem Füllgrad (\~33 % und \~38 %).

> \*\*Wer auf der falschen filtert, produziert Müll, der niemandem auffällt.\*\*



\*\*3. Dem Export fehlen Spalten, die es heute gibt.\*\* Fertigungsversion (seit \~2021) und die EWAHR-gewichtete Menge (seit 2024). Deshalb war die gewichtete Menge nur \*\*in der Transaktion\*\* sichtbar, nicht in der Datei.



> ### → Ein neuer Export gibt 36 saubere Spalten statt 27 verschmolzener. Belegt, nicht vermutet.



\#### Ungelöster Widerspruch zur Mengenspalte

In der SAP-Tabelle steht bei einer Alternative mit Einsatzwahrscheinlichkeit 0 die \*\*rohe\*\* Menge ungleich Null — nur die \*\*gewichtete\*\* ist 0. Der Export hat aber nur \*\*eine\*\* Mengenspalte, und die zeigt \~5.300 Nullen.



\*\*Vermutung (nicht belegt):\*\* Der alte Listendruck zeigte bereits den \*\*gewichteten\*\* Wert. Passt zum Änderungsprotokoll von 2024, das genau die Anzeige der Menge bei Einsatzwahrscheinlichkeit 0 korrigiert und dafür eine eigene Spalte nachgezogen hat.



→ Hängt an der offenen Frage, \*\*welche der drei Mengenspalten\*\* im Export steht.



\#### Drei Selbsttests zur Spaltenidentität (noch nicht durchgeführt)

Falls doch auf dem alten Stand gearbeitet wird — prüfen statt glauben, ohne Fachseite und ohne neuen Export:

1\. \*\*Zeilen mit Dummy-Kennzeichen müssen Elternteile sein\*\* (ein Phantom ist eine Baugruppe mit Kindern). Sind sie Blätter → falsche Spalte.

2\. \*\*Die Einsatzwahrscheinlichkeit muss sich innerhalb einer Alternativgruppe auf 100 summieren\*\* (am System bestätigt).

3\. \*\*Menge 0 muss überwiegend mit Einsatzwahrscheinlichkeit 0 zusammenfallen.\*\*





\---



\### UPDATE 03.09.2026 — Rolle des Legacy-Datensatzes



Der Fachbereich hat den alten Datensatz ausdrücklich als \*\*geeignet für Entwicklung und Tests\*\* eingeordnet, weil bekannte Fehler/Sonderfälle enthalten sind.



Für das finale Arbeitsergebnis gilt jedoch:



> Der spätere Prozess muss mit einem \*\*aktuellen Export aus dem Live-System\*\* beginnen.



Bei einem neuen Export sollten die historischen Listendruckprobleme (zerschossene Kopfzeilen, verschobene Felder etc.) weitgehend behoben sein. Die Legacy-Reparaturen sind deshalb vor allem Rückwärtskompatibilität und Regressionstest, nicht der eigentliche wissenschaftliche Kern.



\#### Aktueller bereinigter Versandstand



Nach gezielter Rekonstruktion von 144 verrutschten Stufenangaben:



```text

117.264  Rohzeilen

&#x20;47.390  saubere Positionszeilen

```



Technische Legacy-Reparaturen im Versandlauf:

\- 1.821 wiederholte Kopfzeilen entfernt

\- 1.766 Leerzeilen entfernt

\- 144 verrutschte Stufenangaben rekonstruiert

\- kaputte Zeichen/Umlaute technisch repariert

\- zerschossene Header für die Ausgabe rekonstruiert/zugeordnet



Fachliche Filter im anschließenden Clean-Lauf:

\- Dummy/Phantom 35/50

\- Positionstyp X entfernt (freigegebener Stand: nur L)

\- Nicht-ST-Einheiten entfernt

\- Schüttgut bleibt enthalten



Von den 144 rekonstruierten Stufen wären 125 ohnehin X und 10 ohnehin Dummy gewesen; \*\*9 zusätzliche fachlich sonst gültige Positionen bleiben dadurch im Ergebnis\*\*.



Die allgemeine Stufen-Recovery ist noch \*\*nicht\*\* Teil des Cores. Für den Legacy-Versand wurde eine separate Einmal-Korrektur vor `bom\_core` verwendet.



\---



\### UPDATE — Diskrepanzen-Blatt: Status NICHT als bestätigte BOM-Fehler festschreiben



Eine neue Detailanalyse durch N kommt auf \*\*17 inhaltliche Fälle\*\* und interpretiert das Blatt als Abgleich zwischen einer Prüf-/Referenzdatenbank und der SAP-Stückliste.



Vorläufige Struktur der 17 Fälle:

\- 6 Fälle: Referenzkomponente in der Stückliste nicht gefunden (`#N/A`) → kein direkter Vergleich möglich

\- 1 Fall: Referenzquelle und Stückliste tragen unterschiedliche Variantenmerkmale; Stückliste ist dabei mit dem Gerätenamen konsistent

\- 10 Fälle: Referenzquelle und Stückliste unterscheiden sich in AC/DC- bzw. Variantenkennzeichnung; N vermutet ein Mapping-/Identifikationsproblem zwischen den Systemen



\*\*Wichtig:\*\* Daraus folgt noch \*\*nicht\*\*, dass die Stückliste fachlich korrekt und die Referenzdatenbank falsch ist.



Saubere Trennung:

\- \*\*Befund:\*\* Gerätename und BOM-Bezeichnung sind in den vergleichbaren Fällen namensseitig konsistent; die Referenzquelle weicht ab.

\- \*\*Hypothese:\*\* unterschiedliches Mapping / unterschiedliche Identifikation desselben funktionalen Bauteils zwischen Systemen.

\- \*\*Offen:\*\* Welche Quelle ist fachlich korrekt? Das braucht Fachseite/Prozesswissen.



Außerdem ist der Widerspruch \*\*17 vs. früher dokumentierte 19 Zeilen/Fälle\*\* zu klären. Bis dahin die frühere Formulierung „19 bestätigte BOM-Fehler / einzige Wahrheitsquelle“ \*\*nicht mehr als gesichert verwenden\*\*.



\---



\### UPDATE 04.09.2026 — ANALYSEBASIS FÜR DEN J-ANSATZ



\#### Aktueller Zeilen- und Erzeugnisstand



| Größe | Wert | Bedeutung |

|---|---:|---|

| Rohzeilen | 117.264 | historischer operativer Export |

| direkter älterer Core-Lauf | 47.381 | ohne einmalige Stufen-Recovery |

| aktueller Analyse-/Versandstand | 47.390 | nach Rekonstruktion von 144 verrutschten Stufenangaben |

| Erzeugnisse im J-Prototyp | 1.692 | Nenner der aktuellen Gruppierungsbasis |



Die Differenz 47.381 → 47.390 ist kein widersprüchlicher Datensatzstand, sondern

eine dokumentierte Korrekturevolution: 9 fachlich sonst gültige Zeilen konnten

aus dem bekannten Legacy-Listendruckartefakt gerettet werden.



\#### Analyseobjekte



Für Phase 1 werden alle bereinigten Positionszeilen eines Erzeugnisses

einschließlich tieferer Ebenen gemeinsam betrachtet. Daraus entstehen innerhalb

einer 6-Zeichen-Hauptgruppe Profile pro ursprünglichem Erzeugnistyp.



Verbindlich:



\- \*\*Materialnummer = Identität einer Komponente\*\*

\- Kurztext = verständliche Darstellung und vorläufige Rollen-/Arterkennung

\- Anwesenheit einer Komponente = Anzahl verschiedener Erzeugnisse mit dieser

&#x20; Materialnummer geteilt durch Anzahl Erzeugnisse des Typs/der Gruppe

\- mehrfache Zeilen desselben Materials in einem Erzeugnis erhöhen den

&#x20; Anwesenheitszähler nicht

\- Mengen werden nicht ungeklärt über Ebenen multipliziert oder summiert

\- Baum, Position, Alternativgruppe und Originalzeilen bleiben zur Rückverfolgung

&#x20; erhalten



\#### J-Abdeckung



Von N auf den echten Daten berichtet:



\- 4 Hauptgruppen: 772 / 512 / 407 / 1 Erzeugnisse

\- 23 auswertbare Typen

\- 15 resultierende Vergleichsgruppen



As Prüfung der \*\*maskierten\*\* Paar-Tabelle ergab Typgrößen mit Summe 1.656.

Damit sind in dieser konkreten Paaransicht 36 der 1.692 Erzeugnisse nicht

erklärt. Denkbare Ursachen sind nicht paarfähige Einzelgruppen, fehlende

Typableitung oder vorherige Filter; die tatsächliche Ursache muss N auf den

internen Daten ausweisen.



\#### Maskierung



Interne und maskierte Ergebnisse müssen:



\- dieselbe Tabellenstruktur,

\- dieselben Zeilenzahlen der aggregierten Analysen,

\- dieselben numerischen Kennzahlen und Entscheidungen



besitzen. Nur Identnummern, Typ-/Materialkennungen und sensible Texte werden

ersetzt bzw. entfernt. A darf aus dem maskierten Stand Strukturen prüfen, muss

aber jeden Befund als \*\*maskiert analysiert\*\* kennzeichnen; N validiert ihn

danach intern erneut.



\#### Zwei noch ungeklärte Datenwirkungen



1\. \*\*Alternativpositionen:\*\* N berichtet 16.129 Zeilen mit Alternativkürzel.

&#x20;  Gelistete Präsenz ist möglicherweise nicht identisch mit tatsächlich

&#x20;  ausgewähltem Verbau und kann Komponentenquoten verzerren.

2\. \*\*Mengen:\*\* Die aktuelle Gruppierung nutzt primär binäre Präsenz. Eine falsche

&#x20;  Menge bei richtiger Materialnummer bleibt dadurch unsichtbar und gehört in

&#x20;  einen späteren, fachlich geklärten Analyseschritt.



\#### Anforderungen an die zentrale Evidenztabelle



Die nächste Ausgabegeneration soll nicht mehrere schwer zusammenlesbare CSVs als

primären Zugang verwenden. Eine Master-Tabelle verbindet Typpaar, Kennzahlen,

konkrete Komponenten, Entscheidung, finale Gruppe, Herkunft der Entscheidung,

Fachurteil, Parameterstand sowie Coverage/Ausschlussgrund. Excel/HTML werden

daraus abgeleitet und rechnen nicht unabhängig neu.



</details>

