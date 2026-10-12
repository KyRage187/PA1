\# G\_HANDOUT — PA1 Big3-Stand und Übergabe, 01.10.2026



> Verfasst von \*\*G\*\* (Corporate-sicherer Builder mit Databricks-Zugang) für \*\*A\*\* (das gesamte ChatGPT-Projekt PA1), weil die vorherigen A-Chats nicht mehr nutzbar sind. Diese Datei ist eine \*\*ergänzende Versuchsdokumentation\*\* zu den neun Projektdateien; sie ersetzt deren Aktualisierung nicht. Die neun Projektdateien wurden parallel als neue Versionen gespeichert (siehe Abschnitt 12).

>

> Statuskennzeichen in dieser Datei: \*\*\[G geprüft]\*\* = von G an Databricks-Ausgaben gesehen; \*\*\[G berichtet]\*\* = von G ausgeführt, Ausgabe aber nicht vollständig in dieser Übergabe; \*\*\[Nutzer entschieden]\*\* = bewusste Arbeitsregel des Bearbeiters; \*\*\[fachlich bestätigt]\*\* = Aussage des Fachbereichs; \*\*\[Hypothese]\*\* = Einschätzung, kein Ergebnis; \*\*\[offen]\*\* = unbeantwortet; \*\*\[Lücke]\*\* = G fehlt die Information.



\---



\## 0. Startanweisung für den nächsten A-Chat



Du bist A. Lies zuerst `00\_START\_HIER` (Version 7), dann diese Datei vollständig, dann `05\_CODE`, `06\_ALGORITHMUS`, `04\_ENTSCHEIDUNGEN\_REGELN`, `FRAGEN`. Der Big2-Handoff vom 28.09.2026 bleibt gültig für Big2; er ist \*\*nicht\*\* durch diese Datei abgelöst, sondern ergänzt.



\*\*Stand in einem Satz:\*\* Big3 existiert seit dem 01.10.2026 als eigenes, lauffähiges Strukturmodell auf GJL121, liest ausschließlich die Cleaner-Ausgabe `sauber`, schlägt Big1 V2 in der Vorhersagegüte, hält den einzigen fachlich bestätigten Big1-Fall oben und erklärt rund 60 der 100 stärksten Big1-Fehlbefunde strukturell. Es ist \*\*vorzeigbar, nicht validiert\*\*: Es gibt genau ein Fachlabel.



\*\*Nächster Schritt (vom Nutzer festgelegt, nicht von G auszuführen):\*\* Ein gemeinsames Notebook, das Cleaner → Big1 → Big2 → Big3 für GJL121 und GJL131 durchläuft und \*\*eine\*\* Anomalieliste für den Fachbereich erzeugt. Dieser Schritt ist noch nicht begonnen.



\*\*Stopppunkt für A:\*\* Keine neuen Big3-Merkmale, keine Hyperparameter, keine neue Hauptgruppe, kein Stacking-Lauf, bevor der Bearbeiter die Weekly-Rückmeldung (geplant 02.10.2026) eingebracht hat. Der Bearbeiter hat ausdrücklich verlangt, dass nicht erneut „ausgeholt“ wird.



\---



\## 1. Was Big3 ist und was es nicht ist



| | Big1 V2 | Big2 | Big3 |

|---|---|---|---|

| Frage | Ist exakte Komponente X in dieser Stückliste zu erwarten? | Ist die Menge/Positionszahl einer \*\*vorhandenen\*\* Komponente plausibel? | Ist X zu erwarten, \*\*wenn man den Aufbau der Stückliste kennt\*\*? |

| Lernzeile | (Fassung, Komponente X), Ziel `vorhanden` 0/1 | (Produkt, Block, Komponente), Ziel Menge/Positionszahl | \*\*identisch mit Big1\*\*: (Fassung, X), Ziel `vorhanden` |

| Eingaben | flacher Kontext (welche anderen Komponenten sind da), Prüfkomponente, Spannung, Nummernbasis, kleinere Gruppe | maskierte Mengen, Nachbarmengen | \*\*alle Big1-V2-Eingaben plus 14 Strukturzahlen\*\* aus dem Cleaner-Baum |

| Folds, Gewichte, Hyperparameter | 5 produktgetrennte Folds, Seed 42+fold, CatBoost 300/6/0,05 | eigene | \*\*von Big1 V2 übernommen\*\* (`get\_params()`) |

| Quelle | `sauber` | `sauber` | `sauber` (seit 01.10.; vorher Excel, verworfen) |

| Status | eingefrorener Benchmark, 1 bestätigter Fall | Pilot, 1 bestätigter Fall | \*\*vorzeigbar, 0 eigene Labels\*\* |



\*\*Big3 ist ein eigenes Modell, kein Stacking.\*\* Eine Stacking-Variante (Big1-V2-Wahrscheinlichkeit als zusätzliche Eingabe) ist im Code als Schalter `BASIS\_B3 = "v2+big1"` vorbereitet, aber \*\*nicht gelaufen\*\* \[Nutzer entschieden: zuerst die „klare Baseline“ ohne Big1-Eingabe, Stacking als späterer Test der „wissenschaftlichen Prüfung“].



\*\*Big3 arbeitet nicht auf der rohen Stückliste.\*\* Es trainiert auf exakt denselben Zeilen und Labels wie Big1 (bereinigter Bestand). Der Cleaner bleibt unangetastet. Die Strukturzahlen nutzen nur Spalten, die der Cleaner selbst erzeugt (`stufe`, `tiefe`, `eltern`, `node`, `slot`, `alt\_gruppe`, `alt\_wahrsch`, `menge`). Das war am 01.10. die zentrale Nutzerkorrektur: \*Alle ML-Modelle müssen auf neuen, nie zuvor gesehenen Stücklisten benutzbar sein; nur der Cleaner kennt den Export.\* \[Nutzer entschieden]



\*\*Abgrenzung zur ursprünglichen Big3-Idee:\*\* Im Big2-Handoff (28.09.) war Big3 als Modell für „fachlich verwandte oder ersetzende Materialnummern und Varianten“ skizziert. Das tatsächlich gebaute Big3 ist ein \*\*Strukturmodell\*\* (Ebenen, Träger-Baugruppen, Stellen, Phantome, Nachbarn). Variantenersetzung (z. B. Gehäuse P0008 ↔ P0009) wird dadurch \*\*nicht\*\* gelöst; siehe Kontrollfall „Gehäuse“ in Abschnitt 5. Die Varianten-/Ersetzungsfrage bleibt eine offene spätere Aufgabe.



\---



\## 2. Die 14 Strukturmerkmale (`STRUKTUR\_B3`)



Alle Merkmale werden \*\*je Fold nur aus Trainingsblöcken\*\* berechnet (kein Leck aus dem Prüffold). Fehlende Werte sind −1. Keine Teilenamen, keine Typlisten, keine Excel-Spaltenindizes.



| Merkmal | Bedeutung | Quelle in `sauber` |

|---|---|---|

| `b3\_traeger\_da` | Die übliche echte Eltern-Baugruppe von X ist im Block vorhanden | `eltern`→`node`→`komponente` |

| `b3\_traeger\_aufgeloest` | Diese Baugruppe ist im Block aufgelöst (hat Kinder) | `eltern` |

| `b3\_traeger\_ist\_erzeugnis` | X hängt üblicherweise direkt am Erzeugnis | `tiefe == 1` |

| `b3\_stelle\_anteil` | Wie eindeutig ist die übliche Stelle von X (Anteil im Training) | `stufe` + `slot` |

| `b3\_stelle\_belegt` | Anzahl \*\*anderer\*\* Komponenten an der üblichen Stelle von X | `stufe` + `slot` |

| `b3\_stelle\_belegt\_menge` | größte Menge einer anderen Komponente an dieser Stelle | `menge` |

| `b3\_stelle\_alpgr` | andere Komponente an der Stelle ist Alternativposition | `alt\_gruppe` |

| `b3\_stelle\_ewahr` | deren Einsatzwahrscheinlichkeit | `alt\_wahrsch` |

| `b3\_x\_alpgr\_anteil` | Anteil der Vorkommen von X als Alternativposition | `alt\_gruppe` |

| `b3\_x\_ewahr\_median` | Median der Einsatzwahrscheinlichkeit von X | `alt\_wahrsch` |

| `b3\_x\_traeger\_phantom\_anteil` | Anteil der Vorkommen, in denen X durch eine entfernte Phantombaugruppe hochgezogen wurde | `stufe > tiefe` |

| `b3\_x\_menge\_median` | Median der Menge von X im Training | `menge` |

| `b3\_nachbar\_anteil\_x` | Anteil der 10 ähnlichsten Trainingsblöcke \*\*anderer Produkte\*\* (Jaccard auf Komponentensets \*\*ohne X\*\*), die X enthalten | Block-Matrix |

| `b3\_nachbar\_jaccard` | mittlere Ähnlichkeit dieser Nachbarn | Block-Matrix |



\*\*Stelle\*\* = `stufe | POS:n` bzw. `stufe | ALT:code`, aus dem Cleaner-Feld `slot` (letztes Element) und der SAP-Stufe `stufe`. Begründung: `eltern\_orig` ist eine Excel-Zeilennummer und blockabhängig, also nicht vergleichbar; die Elternkomponente nach Cleaning ist für 92 % der Zeilen „@ERZEUGNIS“ und damit nicht trennscharf (Lauf 5). Stufe + Position ist je Komponente sehr stabil (`b3\_stelle\_anteil` Mittel 0,936) \[G geprüft].



\*\*Phantom\*\* = `stufe > tiefe`. Der Cleaner entfernt SOBSL-50/35-Zeilen und hängt Kinder an den Großvater (Regel in 02\_SAP\_WISSEN §6). Danach ist die SAP-Stufe größer als die bereinigte Tiefe. In `sauber` kommen in `sobsl\_werte` nur noch 10 und 30 vor; die 50er sind weg \[G geprüft]. Ein SOBSL-Zugriff ist für Big3 deshalb nicht nötig.



\*\*Nachbarn ohne X:\*\* Die Ähnlichkeit wird für jede Prüfkomponente X neu ohne X berechnet (Schnitt − X, Vereinigung − X), damit die Anwesenheit von X nicht in ihre eigene Vorhersage fließt. Nur Trainingsfolds, nie dasselbe Produkt.



\---



\## 3. Chronologie der Big3-Läufe (alle GJL121, 01.10.2026)



Alle Läufe: gleiche Zeilen, gleiche 5 Produkt-Folds, gleiche Hyperparameter wie Big1 V2; 60.219 bewertete Prüfpaare (12.090 / 12.324 / 11.935 / 11.704 / 12.166 je Fold); 774 Blöcke × 103 Komponenten = 79.722 (Block, X)-Paare für die Strukturmerkmale \[G geprüft].



| Lauf | Basis | Strukturquelle | Log-Loss Big3 je Fold (Big1 V2 zum Vergleich: 0,0309 / 0,0276 / 0,0285 / 0,0279 / 0,0308) | Spule 60V (bestätigt) | Joch | Urteil |

|---|---|---|---|---|---|---|

| 1 | V1 | Excel-Parser (eigener Baum) | 0,0366 / 0,0346 / 0,0345 / 0,0350 / 0,0289 | \*\*0,25 (durchgefallen)\*\* | 0,86 | schlechter als V2 in 4/5 Folds; zwei Bugs (siehe 4) |

| 2 | V1 | wie 1, Bugs gefixt | identisch zu 1 | – | – | Zelle war nicht ersetzt worden; kein neuer Erkenntniswert |

| 3 | \*\*V2\*\* | Excel-Parser | \*\*0,0151 / 0,0088 / 0,0095 / 0,0123 / 0,0088\*\* | 0,993 | 0,960 | erster zählender Lauf; Fehler ggü. V2 etwa gedrittelt |

| 4 | V2 | Excel, Kopfzeilen-Suche | 0,0154 / 0,0093 / 0,0093 / 0,0125 / 0,0087 | 0,994 | 0,950 | Spaltensuche griff falsche Spalten (ALPRF statt ALPGR, M.SOBSL statt EWAHR); Ergebnis trotzdem fast gleich, weil CatBoost die falschen Merkmale ignorierte |

| 5 | V2 | \*\*`sauber`\*\*, Stelle = Elternkomponente, Phantom via SOBSL | 0,0192 / 0,0153 / 0,0165 / 0,0169 / 0,0154 | 0,972 | 0,728 | erster Excel-freier Lauf; Träger-Merkmale fast konstant (92 % „am Erzeugnis“), Phantom = 0 |

| \*\*6 (final)\*\* | V2 | \*\*`sauber`\*\*, Stelle = `stufe|POS/ALT`, Phantom = `stufe>tiefe` | \*\*Big1 0,0291 → Big3 0,0136\*\* (alle 5 Folds besser: 0,0168 / 0,0113 / 0,0146 / 0,0140 / 0,0112) | \*\*0,996\*\* | \*\*0,711\*\* | Merkmale korrekt gefüllt, keine konstanten Merkmale; Bearbeiter: „vorzeigbar“ \[G geprüft] |



Brier-Scores Lauf 3: Big1 V2 0,0062 / 0,0053 / 0,0057 / 0,0055 / 0,0067 → Big3 0,0040 / 0,0016 / 0,0022 / 0,0029 / 0,0019. Lauf 5: Big3 0,0051 / 0,0035 / 0,0042 / 0,0043 / 0,0040 \[G geprüft].



\*\*Fold-Vergleich Lauf 6 (final, Databricks-Ausgabe bestätigt):\*\*

\- Fold 0: Big1 = 0,0309, Big3 = 0,0168

\- Fold 1: Big1 = 0,0276, Big3 = 0,0113

\- Fold 2: Big1 = 0,0285, Big3 = 0,0146

\- Fold 3: Big1 = 0,0279, Big3 = 0,0140

\- Fold 4: Big1 = 0,0308, Big3 = 0,0112

\- \*\*Mittelwert: Big1 = 0,0291, Big3 = 0,0136\*\* (Log-Loss mehr als halbiert, Big3 in jedem Fold deutlich überlegen) \[G geprüft].



\### Lauf 6 — Merkmalswichtigkeit (Mittel über 5 Folds) \[G geprüft]



| Merkmal | Wichtigkeit |

|---|---|

| `b3\_nachbar\_anteil\_x` | 36,35 |

| `b3\_stelle\_belegt\_menge` | 5,09 |

| `b3\_x\_traeger\_phantom\_anteil` | 4,17 |

| `b3\_nachbar\_jaccard` | 4,02 |

| `b3\_x\_menge\_median` | 3,43 |

| `b3\_stelle\_anteil` | 2,63 |

| `b3\_traeger\_aufgeloest` | 1,50 |

| `b3\_stelle\_alpgr` | 1,21 |

| `b3\_traeger\_ist\_erzeugnis` | 0,99 |

| `b3\_stelle\_ewahr` | 0,94 |

| `b3\_stelle\_belegt` | 0,77 |

| `b3\_x\_alpgr\_anteil` | 0,43 |

| `b3\_x\_ewahr\_median` | 0,26 |

| `b3\_traeger\_da` | 0,03 |

| stärkste Basismerkmale: `pruefkomponente` 7,73, `komponente\_basis` 7,19, `produkt\_spannung` 5,25 | |



\*\*Einordnung \[G, Hypothese]:\*\* Rund 36 % der Wichtigkeit liegt auf dem Nachbarmerkmal. Big3 ist damit zu einem großen Teil ein „Was haben die zehn ähnlichsten Stücklisten anderer Produkte“-Modell, der Rest ist Struktur. Bei Produkten ohne ähnliche Nachbarn (z. B. R1000-Reihe, Jaccard ≈ 0,13) ist Big3 unsicherer. Das ist eine Grenze, die auf der Fachbereichsliste mitgeführt werden muss.



\### Lauf 6 — Verteilung der Merkmale (describe, letzter Fold) \[G geprüft]



`b3\_traeger\_da` Mittel 0,943 · `b3\_traeger\_aufgeloest` 0,940 · `b3\_traeger\_ist\_erzeugnis` 0,922 · `b3\_stelle\_anteil` 0,936 · `b3\_stelle\_belegt` Mittel 0,13, Median 0, Max 3 · `b3\_stelle\_belegt\_menge` Mittel 0,48, Max 105 · `b3\_stelle\_alpgr` 0,03 · `b3\_stelle\_ewahr` Mittel 3,1 (meist −1, sonst 100) · `b3\_x\_alpgr\_anteil` 0,126 · `b3\_x\_ewahr\_median` 13,6 · `b3\_x\_traeger\_phantom\_anteil` \*\*0,757\*\* · `b3\_x\_menge\_median` 2,56 · `b3\_nachbar\_anteil\_x` 0,226 · `b3\_nachbar\_jaccard` 0,915. Keine konstanten Merkmale.



\### Lauf 6 — Wirkung auf die 100 stärksten Big1-V2-Fehlbefunde \[G geprüft]



| Big3-Auffälligkeit | Anzahl | Lesart |

|---|---|---|

| < 0,2 (stark abgeschwächt) | 33 | strukturell erklärt |

| 0,2–0,5 (abgeschwächt) | 28 | |

| 0,5–0,8 (leicht) | 18 | |

| > 0,8 (bestätigt) | 21 | bleibt Kandidat |



Zusätzlich \*\*23 Fälle\*\*, die Big1 V2 unter 0,5 hatte und Big3 über 0,8 bewertet („Gegenrichtung“). Typen: Stößel, Verpackung (Schachtel, Klebeschild), Rückstellfedern 0,4/0,5/0,6 N, Abdeckungen, Festschaltstücke/Kontaktbrücke in zwei R1000-Produkten, eine 6-V-Spule, Klebeetiketten. \*\*Keiner dieser Fälle ist fachlich geprüft.\*\* Die konkreten Kennungen stehen im internen Anhang A.



\---



\## 4. Fehlschläge und Korrekturen (für C: gehören ins Versuchsregister)



1\. \*\*Excel-Parser als zweite Datenquelle (Läufe 1–4) — verworfen \[Nutzer entschieden].\*\* G hatte den Rohexport direkt mit `pd.read\_excel` gelesen und einen eigenen Stufen-Baum gebaut, mit festen Spaltenindizes, dann Kopfzeilen-Suche, dann Inhaltsregeln. Alle drei Varianten hingen vom konkreten Exportlayout ab (Kopfzeile gegenüber Datenzeilen ab Spalte 10 um eins verschoben; Umlaute in Kopfzeilen beschädigt). Der Bearbeiter hat das als Verstoß gegen das Projektprinzip gestoppt: \*Nur der Cleaner liest den Export; alle Modelle lesen `sauber`.\* Big3 wurde daraufhin vollständig auf `sauber` umgestellt (Lauf 5/6). Der Excel-Lauf 3 bleibt als Vergleichswert dokumentiert, ist aber \*\*nicht\*\* der gültige Stand.

2\. \*\*Zwei Bugs im Excel-Baum (Lauf 1):\*\* (a) Elternzeile als float, SOBSL-Index als int → Mapping lieferte NaN → Phantom-Anteil konstant 0; (b) Kopfzeile des Exports landete als Elternteil auf dem Stapel → Stufe-1-Teile hatten „Materialkurztext Teil“ als Träger. Beide behoben, danach durch die `sauber`-Umstellung ohnehin obsolet.

3\. \*\*Spule-60V-Fehlschlag (Lauf 1):\*\* Mit V1-Basis (ohne `produkt\_spannung`) fiel der bestätigte Fall von 0,999 auf 0,25, weil die zehn Nachbarn andere Spannungen hatten. Mit V2-Basis (Lauf 3 ff.) blieb er stets über 0,97. Schluss: Big3 \*\*muss\*\* auf V2 aufsetzen; V1 als Basis ist nicht zulässig.

4\. \*\*Falsches Label „Klebeetikett ITAL. BAHN – bekannt echt“ (G-Fehler):\*\* G hatte einen Big1-Befund als bekannt bezeichnet; der Bearbeiter hat das nie gesagt. Korrigiert zu „unbestätigt“. \*\*Einziger fachlich bestätigter Fall ist die Spule 60V in einem GJL121-Produkt\*\* (historischer Big1-Spitzenfall, siehe Big2-Handoff).

5\. \*\*Variablenfehler in G-Zellen (`ktext` statt `komponentennamen`, fehlende `hauptgruppe`-Spalte, Spaltenindizes):\*\* Der Bearbeiter musste mehrere Zellen durch einen Assistenten reparieren lassen. Regel seit dem 01.10.: G schreibt an den Anfang jeder Zelle die vorausgesetzten Variablen und nutzt nur Namen aus dem aktuellen Notebook.

6\. \*\*Joch nicht „gelöst“ und bewusst nicht erzwungen \[Nutzer entschieden]:\*\* Der Bearbeiter hat G untersagt, die Nachbarbildung auf den Joch-Fall zuzuschneiden („wir forcieren hier gerade, dass das Modell das findet“). Joch bleibt Kontrollfall; ob eine nicht aufgelöste Baugruppe ein Fehler ist, sagt der Fachbereich.

7\. \*\*Wiederholtes Ausholen/Erklären (Kommunikationsfehler G):\*\* Der Bearbeiter hat mehrfach klargestellt, dass er keine erneuten Gesamterklärungen will, sondern konkrete Ersetzungsanweisungen („was wo weg muss“) und erst dann eine Erklärung, \*\*wie\*\* Big3 funktioniert, wenn es funktioniert.



\---



\## 5. Kontrollfälle (Lauf 6) \[G geprüft]



| Fall (neutral) | vorhanden | Big1 V2 | Big3 | Stärkster Strukturbeitrag | Bewertung |

|---|---|---|---|---|---|

| Spule 60 V fehlt — \*\*fachlich bestätigter Fehler\*\* | 0 | 0,9991 | \*\*0,9964\*\* | Nachbarn (−1,54 auf P(da)) | muss oben bleiben → erfüllt |

| Joch fehlt, geschweißte Joch-Baugruppe vorhanden, nicht aufgelöst | 0 | 0,9872 | \*\*0,7108\*\* | `traeger\_aufgeloest` −2,69 | richtige Richtung, richtige Begründung; offen, ob fachlich Fehler |

| Gehäuse-Variante P0008 fehlt, P0009 vorhanden (Produkt mit Geschwisterblock) | 0 | 0,9914 | 0,9957 | — | \*\*nicht gelöst\*\*; Variante kommt nur 4× vor |

| Gehäuse-Variante, Produkt ohne Geschwisterblock | 0 | 0,9954 | 0,7179 | `stelle\_ewahr` | teilweise |

| Rückstellfeder 0,4 N fehlt, 0,3 N vorhanden | 0 | 0,9395 | 0,6614 | `x\_menge\_median` | Bauformfrage; Big2-Gebiet, nicht Big3 |

| Hilfskontaktbrücke fehlt (Bauform 3+1 vs. 4) | 0 | 0,9981 | \*\*0,2130\*\* | `stelle\_belegt\_menge` −1,91 | strukturell erklärt |

| Klebeetikett fehlt — \*\*unbestätigt\*\* | 0 | 0,9985 | 0,9984 | — | bleibt Kandidat |



Interne Kennungen zu diesen Fällen: Anhang A.



\---



\## 6. Datenbefunde aus dieser Phase \[G geprüft, GJL121]



| Zahl | Einheit | Quelle |

|---|---|---|

| 47.390 | bereinigte Positionszeilen, gesamter Bestand | `clean\_bom()`, Cmd 3 (Assert im Notebook) |

| 20.066 | bereinigte Positionszeilen in GJL121 | `sauber`, Cmd 21 |

| 774 | Produktblöcke GJL121 | `block\_sets` |

| 103 | Komponenten in der Big1-Matrix GJL121 | `X\_pilot.columns` |

| 5.319 | Zeilen mit `tiefe > 1` in GJL121 (echte Unterbaugruppen nach Cleaning) | Cmd 21, Lauf 5 |

| 7.160 | Zeilen mit `alt\_gruppe` in GJL121 | Cmd 21, Lauf 5 |

| 44.234 / 12.642 / 4.358 | Rohzeilen Typ L / davon SOBSL-50/35-Zeilen / Stufe-1-Zeilen in GJL121 \*\*(historisch, aus dem verworfenen Excel-Parser)\*\* | Cmd 23 alt |

| `sobsl\_werte` in `sauber` | nur 10 (8.445) und 30 (1.258), Rest leer (37.687); keine 50/35 | Lesezelle 01.10. |



\*\*Cleaner-Spalten in `sauber` (vollständige Liste, 01.10.2026):\*\* `\_roh, \_weg\_kopie, alt\_gruppe, alt\_rang, alt\_wahrsch, block, eltern, eltern\_orig, eltern\_teil, erzeugnis, erzeugnis\_txt, komponente, komponente\_txt, me, menge, node, posnr, postp, retrograd, schuettgut, slot, sobsl\_werte, stufe, teil\_id, tiefe, zeile\_excel`.



\*\*Bedeutung der Strukturfelder (am Beispielprodukt gesehen):\*\* `stufe` = SAP-Stufe vor Cleaning (z. B. Joch 5, Gehäuse 1); `tiefe` = Ebene nach Hochziehen (Joch 1, wenn Phantom entfernt; 2, wenn unter echter geschweißter Baugruppe); `eltern` = Knoten der echten Elternzeile oder `@ERZ <Produkt>#<Block>`; `eltern\_orig` = Excel-Zeile der ursprünglichen (auch Phantom-)Elternzeile; `slot` = `<eltern\_orig> | POS:n` oder `| ALT:<Code>`; `alt\_wahrsch` = Einsatzwahrscheinlichkeit, bei allen gesehenen Alternativpositionen 100 (keine allgemeine Regel).



Ergänzung zu 02\_SAP\_WISSEN: Die Einsatzwahrscheinlichkeit (`alt\_wahrsch`/EWAHR) wurde erstmals als Modellmerkmal getestet; Wichtigkeit 0,94 bzw. 0,26 → geringer Beitrag auf GJL121. Keine fachliche Aussage daraus.



\---



\## 7. Vom Nutzer am 01.10.2026 getroffene Regeln \[Nutzer entschieden]



1\. \*\*Universalität vor allem anderen:\*\* Jedes Modell muss auf neuen, nie gesehenen Stücklisten laufen. Nur der Cleaner darf das Exportformat kennen. Kein `read\_excel`, keine Spaltenindizes, keine Kopfzeilen-Suche in Modellcode.

2\. \*\*Erst Baseline ohne Big1-Eingabe, dann Stacking als Test\*\* („klare Baseline“).

3\. \*\*Kein Zuschneiden des Modells auf Einzelfälle\*\* (Joch).

4\. \*\*Nur bestätigte Labels als bestätigt bezeichnen.\*\* Aktuell genau eines (Spule 60 V).

5\. \*\*Dummys/Phantome sind kein Fehler\*\*, sondern normale SAP-Gliederung; der Cleaner entfernt sie korrekt. Big3 nutzt die Information „wurde hochgezogen“ als Merkmal, nicht als Befund.

6\. \*\*Arbeitsweise:\*\* G liefert komplette Ersetzungszellen mit Cmd-Nummer, keine Suchanweisungen; keine erneuten Gesamterklärungen; Erklärung des „Wie“ erst, wenn das Modell funktioniert.

7\. \*\*Nächster Schritt ist nicht G:\*\* Das gemeinsame Notebook mit einer Liste für den Fachbereich macht der Bearbeiter selbst bzw. ein anderer Agent. G liefert nur noch Big3-Ergänzungen.

8\. \*\*Handoff nur auf Anweisung\*\* (dieses Dokument).



\---



\## 8. Offene Punkte



\*\*Daten/Code (G/Bearbeiter):\*\*

\- \[erledigt] Fold-Vergleich (Block 1) des finalen Laufs 6 ergänzt (Mittelwert 0,0291 → 0,0136). \[G geprüft]

\- \[offen] Stacking-Lauf `BASIS\_B3 = "v2+big1"` — Code vorhanden, nicht ausgeführt.

\- \[offen] Big3 auf GJL131 — nur `HAUPTGRUPPE` in Cmd 8 ändern; nicht gelaufen.

\- \[erledigt] Die Anzahl „hochgezogen“-Zeilen (`stufe > tiefe`) beträgt \*\*43.251\*\* von 47.390 Zeilen (91,3 %). \[G geprüft]

\- \[offen] Gemeinsames Notebook Cleaner → Big1 → Big2 → Big3 → eine Liste (GJL121 + GJL131).

\- \[offen] Ausgabeformat von Big3 für die Liste: `p\_big3`, `auff\_big3`, zwei stärkste Strukturbegründungen je Zeile (vorgeschlagen, nicht umgesetzt).

\- \[offen] Big3-Modelle werden derzeit \*\*nicht gespeichert\*\* (kein `.cbm`, kein Manifest). Für TRAIN/INFER-Trennung analog Big2 nötig.



\*\*Fachbereich (Weekly 02.10.2026):\*\*

\- Gruppe A „beide hoch“: Spule 60 V (bestätigt), Klebeetikett, Stößel → Fehler?

\- Gruppe B „Big1 hoch, Big3 niedrig“: Joch (nicht aufgelöst), Hilfskontaktbrücke, Feder 0,3 N statt 0,4 N → gültige Variante oder Fehler?

\- Gruppe C „nur Big3 hoch“: die 23 neuen Fälle → Fehler?

\- Ist eine im Block vorhandene, aber nicht aufgelöste geschweißte Baugruppe ein Stücklistenfehler oder zulässig?

\- Gilt Gehäuse P0009 als gleichwertiger Ersatz für P0008?



\*\*Methodisch \[Hypothese/offen]:\*\*

\- Dominanz des Nachbarmerkmals (36 %): Ist Big3 bei Produkten ohne ähnliche Nachbarn verlässlich genug für eine Liste?

\- Varianten/Ersetzungen (ursprüngliche Big3-Idee) sind weiterhin unbearbeitet.

\- Reduziert Big3 den Prüfaufwand messbar? Erst mit Labels beantwortbar.



\---



\## 9. Was C dokumentieren soll (Praxisarbeit, Abgabe 16.10.2026)



Neu seit 28.09.: Big3-Hypothese (Big1-Fehlbefunde durch Struktur erklärbar) → Erkundung an Joch/Geschweisst → Excel-Parser-Ansatz und sein Scheitern an der Universalitätsanforderung → Umstellung auf Cleaner-Ausgabe → finaler Lauf mit 14 Strukturmerkmalen → Wirkung auf Big1-Top-100 → Kontrollfälle → Grenzen (ein Label, Nachbar-Dominanz, Varianten ungelöst).



Korrigierte frühere Aussagen: „Big3 = Varianten-/Ersetzungsmodell“ (28.09.) → tatsächlich gebaut: Strukturmodell. Big3 „nicht trainiert“ (28.09.) → seit 01.10. trainiert auf GJL121, nicht gespeichert.



Verwendbar für den Kapitelentwurf: Tabelle Big1/Big2/Big3 (Abschnitt 1), Merkmalstabelle (2), Laufchronologie (3), Kontrollfälle (5). Fehlende Abbildung: Fold-Vergleich Lauf 6; SHAP-Beispiel für einen Kontrollfall.



\---



\## 10. Code-Stand (Notebook `MAIN\_CatBoost1`, Databricks, Stand 01.10.2026)



| Cmd | Inhalt | Status |

|---|---|---|

| 1–3 | Install, Dateien, `bc.clean\_bom()` → `sauber` (Assert 47.390) | Big1, unverändert |

| 4–7 | Blöcke, Fassungen, Hauptgruppen, `X\_je\_hauptgruppe` | Big1, unverändert |

| 8 | `HAUPTGRUPPE = "GJL121"`, Seed 42, 5 Produkt-Folds | Big1 |

| 9–13 | Paare, Kontextmaskierung, Big1 V1 (CatBoost 300/6/0,05), Häufigkeitsbaseline, Auffälligkeit | Big1 |

| 14–15 | V2-Merkmale (Spannung, Nummernbasis, kleinere Gruppe), Big1 V2 | Big1 |

| 16–18 | Spitzenlisten, Pflichtspalten, Spaltenbericht `komponentennamen` | Big1 |

| 19–20 | Big3-Erkundung (Joch-Hypothese; „X in vorhandener Baugruppe Z“) | nur lesen, historisch |

| \*\*21\*\* | \*\*Big3 M1: 14 Strukturmerkmale je Fold aus `sauber`\*\* | gültig (Lauf 6) |

| \*\*22\*\* | \*\*Big3 M2: Training je Fold, Vergleich mit Big1 V1/V2, Wichtigkeit, Top-100, Gegenrichtung, Kontrollfälle mit SHAP; Schalter `BASIS\_B3`\*\* | gültig |

| 23 | Lesezelle `sauber`-Strukturfelder | löschen |



Gelöscht am 01.10.: ehemalige Cmd 21–23 (Rohdaten-Check mit `read\_excel`, SAP-Stellenvergleich, Excel-Baum). Der Code von Cmd 21/22 steht vollständig im Databricks-Notebook und im HTML-Export `MAIN\_CatBoost1 (3).html`; ergänzt um die in Abschnitt 11 genannte letzte Änderung.



Reproduzierbarkeit: Big3 benötigt alle Big1-Objekte derselben Sitzung (`paare`, `features\_v2`, `modelle\_v2`, `modell\_spalten\_v2`, `kategoriale\_spalten\_v2`, `ergebnisse`, `ergebnisse\_v2`, `komponentennamen`, `block\_sets`, `fassungen`, `X\_pilot`, `fold\_je\_produkt`). Keine Dateien werden geschrieben.



\---



\## 11. Letzte Code-Änderung (Lauf 6 gegenüber HTML-Export (3))



Der HTML-Export `MAIN\_CatBoost1 (3).html` zeigt Cmd 21 im Stand von Lauf 5. Für Lauf 6 wurden in Cmd 21 ersetzt: `ist\_phantom` (SOBSL) → `hochgezogen = stufe > tiefe`; `stelle = eltern\_komp` → `stelle = stufe | letztes Element von slot`; `stelle\_rows` gruppiert nach `(bkey, stelle)`; in `struktur\_fuer\_fold` wird der Träger als häufigste `eltern\_komp` je X getrennt von der Stelle bestimmt. Die vollständige ersetzte Zelle wurde dem Bearbeiter im Chat übergeben und ist im Databricks-Notebook gespeichert \[G berichtet; kein HTML-Export dieses Stands in dieser Übergabe].



\---



\## 12. Speicherstatus dieser Übergabe



G kann nicht in das PA1-Projekt schreiben. Erzeugt wurden lokal:



| Datei | Ersetzt |

|---|---|

| `17-00\_START\_HIER-7.md` | `17-00\_START\_HIER-6-1-(2).md` |

| `13-AGENTEN\_ROLLEN-6.md` | `13-AGENTEN\_ROLLEN-5-(2).md` |

| `15-14-01\_PROJEKT-6.md` | `15-14-01\_PROJEKT-5-1-(2).md` |

| `11-02\_SAP\_WISSEN-5.md` | `11-02\_SAP\_WISSEN-4-(3).md` |

| `10-04\_ENTSCHEIDUNGEN\_REGELN-5.md` | `10-04\_ENTSCHEIDUNGEN\_REGELN-4-(1).md` |

| `08-05\_CODE-6.md` | `08-05\_CODE-5-(1).md` |

| `18-13-06\_ALGORITHMUS-5.md` | `18-13-06\_ALGORITHMUS-4-1-(1).md` |

| `16-FRAGEN-4.md` | `16-FRAGEN-3-2-1-(2).md` |

| `G\_HANDOUT\_Big3\_2026-10-01.md` | neu (diese Datei) |



Unverändert bleiben: `03\_DATENSATZ.md` (lag G nicht vor — \*\*\[Lücke]\*\*; die Datenbefunde aus Abschnitt 6 müssen dort von A nachgetragen werden), `PA1\_BIG2\_Handoff\_2026-09-28.md`, `14-09\_REVERSE\_ANSATZ\_STAND\_PRUEFPLAN(2).md`, alle 07/08/09/10-Historiendateien.



\*\*Der Upload in PA1 steht aus und muss vom Bearbeiter erfolgen.\*\* Ein Download allein aktualisiert die Project Files nicht.



Bekannte Lücken dieser Übergabe: Block 1 Lauf 6; Weekly-Ergebnisse ab 02.10.; alles, was A zwischen 28.09. und 01.10. außerhalb dieses G-Chats getan hat; `03\_DATENSATZ.md` nicht aktualisiert; exakte Wortlaute früherer A→N-Aufträge.



\---



\## Anhang A — INTERN (Materialnummern; vor Upload in ein externes Projekt prüfen, ob sie dort stehen dürfen)



Kontrollfälle (`KONTROLLE\_B3` in Cmd 22): Joch `GJL1213503R9006 / GJL1203101P0001` (Fassung 1172); Gehäuse mit Geschwisterblock `GJL1213909R1016 / GJL1206624P0008` (1246, 1247); Gehäuse ohne Geschwisterblock `GJL1213903R9005 / GJL1206624P0008` (1226); Spule 60 V \*\*bestätigt\*\* `GJL1213001R8015 / GJL1201344R0013` (918); Feder 0,4 N `GJL1213001R8015 / GJL1203609P0001` (918); Hilfskontaktbrücke `GJL1213403R7005 / GJL1203301P0003` (1142); Klebeetikett unbestätigt `GJL1213001R9033 / 2CDN368507P0001` (922). Geschweißte Joch-Baugruppe: `GJL1201005R0004`.



Die 23 „nur Big3 hoch“-Fälle (Lauf 6, Big3 > 0,8, Big1 < 0,5), sortiert nach Big3: R9089/GJL1206609P0003 Stößel 0,993; R9065/2CDN368502P0006 Klebeschild 0,993; R9065/2CDN361181P0111 Schachtel 0,967; GJL1211901R7012/GJL1203604P0001 Feder 0,6N 0,965; R9031/GJL1206610P0008 Abdeckung grau 0,963; GJL1213503R0006/GJL1201344R0016 Spule 6V DC 0,959; GJL1211909R7012/Feder 0,6N 0,951; GJL1213913R0101/2CDN368507P0001 Klebeetikett 0,931; GJL1211901R7012/GJL1203606P0001 Feder 0,5N 0,929; GJL1213913R0101/Stößel 0,912; GJL1213003R8011/Feder 0,4N 0,889; GJL1211003R9088/Schachtel 0,860; GJL1211009R7102/Feder 0,4N 0,851; GJL1211203R9014/Klebeetikett 0,826; R9031/GJL1206642P0002 Doppelabdeckung 0,826; GJL1211009R1000 und GJL1213009R1000 jeweils Festschaltstück links/rechts (GJL1201331R0007/R0008) 0,825 und Kontaktbrücke GJL1201332R0002 0,821; GJL1213209R5001/Feder 0,4N 0,822; GJL1211003R9018/Feder 0,4N 0,818.

