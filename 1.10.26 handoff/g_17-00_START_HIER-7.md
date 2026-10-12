# 00 — START HIER

> **Aktueller Stand: 01.10.2026 (von G geschrieben, A-Chats nicht verfügbar).** Das gesamte Projekt PA1 ist A; ein neuer Chat ist eine weitere Sitzung. G ist der Corporate-sichere Builder mit Datenzugang; C dokumentiert. Verbindliche ausführliche Übergabe für Big3: [G_HANDOUT](G_HANDOUT_Big3_2026-10-01.md). Für Big2 gilt weiterhin der [Big2-Handoff](PA1_BIG2_Handoff_2026-09-28.md).

## Aktuelle Aufgabe und Stopppunkt (01.10.2026)

**Big3 existiert** seit dem 01.10.2026 als eigenes Strukturmodell auf GJL121. Es nutzt dieselben Zeilen, Folds, Gewichte und Hyperparameter wie Big1 V2 und ergänzt 14 Strukturmerkmale (Träger-Baugruppe, Stelle = SAP-Stufe + Position/ALT, Phantom-Herkunft, Nachbarblöcke ohne X), die ausschließlich aus der Cleaner-Ausgabe `sauber` berechnet werden. Kein Excel-Zugriff im Modellcode. Ergebnis [G geprüft]: Vorhersagefehler außerhalb des Trainings in allen fünf Folds unter Big1 V2 (Lauf 5: 0,029 → 0,016 Log-Loss; Lauf 3 mit später verworfener Excel-Quelle 0,011); der einzige fachlich bestätigte Big1-Fall (Spule 60 V) bleibt bei 0,996; 61 der 100 stärksten Big1-Fehlbefunde werden strukturell abgeschwächt, 21 bestätigt, 23 neue Kandidaten kommen hinzu. Der Bearbeiter hat den Stand als **vorzeigbar** eingestuft. Es gibt **keine** Big3-eigenen Fachlabels, der Fold-Vergleich des finalen Laufs fehlt in der Übergabe (Lücke).

Big3 ist **kein** Stacking auf Big1; der Stacking-Test (`BASIS_B3 = "v2+big1"`) ist vorbereitet, nicht gelaufen. Big3 löst Variantenersetzung (z. B. Gehäuse P0008/P0009) **nicht**; die ursprüngliche Big3-Idee „Varianten/Ersetzungen“ bleibt offen.

**Nächster konkreter Schritt (vom Bearbeiter festgelegt, nicht durch G):** Ein gemeinsames Notebook Cleaner → Big1 → Big2 → Big3 für GJL121 und GJL131, das **eine** Anomalieliste für den Fachbereich erzeugt. **Stopppunkt:** keine neuen Merkmale, Parameter, Hauptgruppen oder Stacking-Läufe für Big3, bevor die Weekly-Rückmeldung (02.10.2026) vorliegt. Der Big2-Stopppunkt (systematische Injektion vor neuen Merkmalen) gilt unverändert.

**Nutzerregeln vom 01.10.:** Nur der Cleaner kennt das Exportformat; alle Modelle lesen `sauber`. Erst Baseline ohne Big1-Eingabe, dann Stacking. Kein Zuschneiden auf Einzelfälle. Nur bestätigte Labels als bestätigt nennen. Komplette Ersetzungszellen statt Suchanweisungen; keine erneuten Gesamterklärungen.

## Lesereihenfolge

**00 → G_HANDOUT → Big2-Handoff → 05_CODE → 03_DATENSATZ → 04_ENTSCHEIDUNGEN_REGELN → 06_ALGORITHMUS → FRAGEN.** 01_PROJEKT und AGENTEN_ROLLEN für Ziel und Zusammenarbeit, 02_SAP_WISSEN für Fach-/Exportgrenzen. **Abgabeziel: 16.10.2026.**

---

## Historischer Stand 28.09.2026 (Big2-Handoff) — Big2-Teile weiterhin gültig, Big3-Aussagen abgelöst

### Aufgabe und Stopppunkt vom 28.09.2026 (Big2: gültig; „Big3 zurückgestellt“: abgelöst am 01.10.)

Big1 V2 bleibt eingefrorener Anwesenheits-Benchmark mit einem fachlich bestätigten historischen Spitzenfall. **Big2** bewertet vorhandene Komponenten und ihre Mengen auf bereinigter Tiefe 1. In GJL121 und GJL131 laufen getrennte fünf Fold-Modelle; die Variante mit fünf datengetriebenen Nachbarschaftsmerkmalen ist der Standard für die nächste Evaluation. Ein zweiter fachlich bestätigter Fall, ein halbierter Produktblock, liegt nach Aggregation auf Produktrang 1 in GJL131. G meldet vier von vier gefundenen Handinjektionen. Das ist ein guter Pilotbefund, noch keine allgemeine Recall- oder Precision-Zahl.

**Nächster konkreter Schritt (28.09., für Big2 weiterhin offen; die Rückstellung von Big3 darin ist seit 01.10. abgelöst):** G soll die systematische Injektion gegen die saubere, eingefrorene Trainings- und Nachbarreferenz mit geladenen Modellen ausführen, ohne neue `.fit()`-Läufe. Pro Hauptgruppe und Fehlertyp Recall@5/10, Zeilen- und Produktränge, Falschalarmrate und Abdeckung mit Nennern ausweisen. Den beobachteten produktionsnahen Handversuch davon trennen. Weitere Merkmale, Hauptgruppen, Komponentengruppen und Big3 sind bis zur Auswertung zurückgestellt; Fachlabels für ausgewählte Kandidaten bleiben parallel erwünscht.

Die gespeicherten Modelle decken nur die beiden Pilot-Hauptgruppen und die aktuelle Einheit `(Produkt, Block, Komponente)` auf Tiefe 1 ab. Neue Hauptgruppen, neue Produkte, Nullmengen, alternative Komponenten und tiefere Elternkontexte brauchen explizite Prüfung. Auffälligkeit ist eine Ranking-Größe und keine Wahrscheinlichkeit eines echten Fehlers.

### Lesereihenfolge vom 28.09.2026 (abgelöst durch oben)

**00 → Big2-Handoff → 05_CODE → 03_DATENSATZ → 04_ENTSCHEIDUNGEN_REGELN → 06_ALGORITHMUS → FRAGEN.** Für Projektziel und Zusammenarbeit: 01_PROJEKT, AGENTEN_ROLLEN. 02_SAP_WISSEN liefert Fach-/Exportgrenzen. Historische Rollen-/NMF-Versuche sind in 07/08 und dem alten Inhalt der Dateien dokumentiert; 09 ist die frühe Reverse-Skizze, 10 dokumentiert Big1 V2. Der GitHub-Code und Databricks-Snapshot sind im Big2-Handoff eindeutig bezeichnet; A hat die Corporate-Outputs nicht selbst gesehen.

**Abgabeziel: 16.10.2026.** C benötigt den Versuchspfad einschließlich negativer Versuche, der Korrekturen, Quellen und Grenzen. Keine automatischen Fachurteile oder externen Nachrichten aus dieser Übergabe ableiten.

---

## Historische Projektstände (09.–16.09.2026; keine aktuellen Aufträge)

Die folgenden Abschnitte bewahren frühere Befunde. Alle Formulierungen „nächster Schritt“, „aktueller Stand“ und „Stopppunkt“ darin gehören zu ihrem angegebenen Datum. Bei Widerspruch gilt der oben datierte aktuelle Stand.

## Historischer Anschluss vom 09.09.2026 — abgelöst

Nicht R1 neu starten. R1 und der begrenzte Positionstest R1.1 liegen vor und wurden anhand der verfügbaren maskierten Codes und CSVs geprüft. Ihre gemeinsame Grenze ist dokumentiert: Die aus dem bereinigten Aufbau abgeleitete Umgebung ist zu flach; die gewöhnliche Positionsnummer ist kein brauchbares Rollensignal. Vorhandene SAP-Alternativgruppen funktionieren als gepflegtes Rollenwissen, decken aber nur einen Teil des Bestands ab.

Danach wurde in Databricks ein **getrennter NMF-Pilot** auf der Hauptgruppe GJL121 durchgeführt. Der Pilot lernt keine Komponentenrollen und ist kein Anomaliedetektor. Er zerlegt eine binäre Produkt-Komponenten-Matrix in additive Komponentenmustern und vergleicht anschließend die durchschnittlichen Musterprofile bestehender kleinerer Gruppen.

**Nächster einzelne Schritt:** Den tatsächlich ausgeführten NMF-Stand reproduzierbar sichern: Notebook beziehungsweise vollständige Trainingszelle, Parameter, `nmf_ergebnisse`, die rohen Matrizen W und H samt Produkt-/Komponentenreihenfolge und idealerweise die Modelle oder erneuerbare Seeds exportieren. Danach zuerst H fachlich ansehen. Noch keine optimale Musteranzahl, endgültige Zusammenführung kleinerer Gruppen oder Anomalieentscheidung behaupten.

## Lesereihenfolge

Für den schnellen Einstieg: **00 → AGENTEN_ROLLEN → 04 → 09 → 06 → 03 → 05 → FRAGEN**. Für Historie und Fachkontext zusätzlich 01, 02, 07 und 08 lesen.

| Datei | Ausführliche Heimat |
|---|---|
| [AGENTEN_ROLLEN.md](13-AGENTEN_ROLLEN-5-%282%29.md) | A/N/C, verbindliche Sprache, Zusammenarbeit und Quellenstatus |
| [01_PROJEKT.md](15-14-01_PROJEKT-5-1-%282%29.md) | Ziel, drei Aufgaben, Versuchsgeschichte und Praxisarbeit |
| [02_SAP_WISSEN.md](11-02_SAP_WISSEN-4-%283%29.md) | SAP-Kontext und Grenzen der Struktur-/Positionsinterpretation |
| [03_DATENSATZ.md](12-03_DATENSATZ-5-%282%29.md) | Gesamtbestand, R1-Basis und NMF-Pilotmenge GJL121 |
| [04_ENTSCHEIDUNGEN_REGELN.md](10-04_ENTSCHEIDUNGEN_REGELN-4-%281%29.md) | Gültige Entscheidungen, Ausschlüsse und Stopppunkt |
| [05_CODE.md](08-05_CODE-5-%281%29.md) | Tatsächlich vorhandener Code, Outputs, Prüfungen und fehlende NMF-Artefakte |
| [06_ALGORITHMUS.md](18-13-06_ALGORITHMUS-4-1-%281%29.md) | R1/R1.1-Erkenntnisse, NMF-Methode, Alternativen und Grenzen |
| [FRAGEN.md](16-FRAGEN-3-2-1-%282%29.md) | Aktuelle Reverse-Fragen und historisches Register |
| [07_ROLLENMODELL_R1_AUFTRAG_PRUEFPLAN.md](06-07_ROLLENMODELL_R1_AUFTRAG_PRUEFPLAN-2-.md) (historisches Artefakt) | Unveränderter R1-Originalauftrag plus Ergebnisstatus und A-Prüfung |
| [08_NMF_PILOT_STAND_PRUEFPLAN.md](09-08_NMF_PILOT_STAND_PRUEFPLAN-2-.md) | Vollständiger NMF-Pilotstand, Formeln, Ergebnisse, Begründung und nächster Prüfplan |
| [09_REVERSE_ANSATZ_STAND_PRUEFPLAN.md](14-09_REVERSE_ANSATZ_STAND_PRUEFPLAN%282%29.md) | Historische Reverse-Skizze; Big2-Anschluss oben und im Handoff |

## Projektbild vom 09.09.2026 — Versuchsgeschichte

1. Eingabe ist eine **Stückliste** mit Produkten und Komponenten.
2. Produkte mit denselben ersten sechs Stellen der Produkt-Identnummer bilden eine **Hauptgruppe**. Hauptgruppen werden niemals vermischt.
3. Innerhalb einer Hauptgruppe bestehen **kleinere Gruppen**, beispielsweise B6, B6S oder VB6A.
4. Frühere Rollenversuche fragten, ob unterschiedliche Komponenten im Aufbau dieselbe Aufgabe erfüllen könnten. R1/R1.1 zeigten verwertbare Teilbefunde, aber keine ausreichend scharfe automatische Rollenbildung.
5. Der neue NMF-Pilot verfolgt einen anderen Weg: Produkte werden direkt über Mischungen gelernter Komponentenmustern beschrieben. Erst danach werden kleinere Gruppen über gemittelte Produktprofile verglichen.
6. Eine hohe mathematische Ähnlichkeit ist nur ein Zusammenführungsvorschlag, keine Wahrscheinlichkeit fachlicher Richtigkeit.
7. Anomalieerkennung bleibt eine spätere eigene Aufgabe.

## Belegter Stand der historischen R1/NMF-Phase

| Aussage | Status und Quelle |
|---|---|
| `clean_bom()` als gemeinsame Eingangsstufe; 47.390 bereinigte Positionszeilen und 1.692 Produkte | Von N berichtet und in späteren Läufen bestätigt; A hat den aktuellen Corporate-Rohdatensatz nicht selbst neu berechnet |
| Hauptgruppen nach ersten sechs Stellen der Produkt-Identnummer, niemals mischen | Fachlich bestätigt / vom Bearbeiter gesetzt |
| R1-v1: 16.419 Komponentenpaare; 5.649 K3-Paare; 87,1 % mit messbarer Strukturähnlichkeit | N-Bericht; A hat `r1_rollen.py` und die maskierte CSV mit 16.419 × 26 geprüft |
| R1-Strukturdomination: 14.031 von 14.299 Paaren mit Ähnlichkeit nur über Nachbarschaft; daraus keine Rollenwahrheit | N-Bericht und A-Dateiprüfung; Nachbarschaft entspricht im flachen Bestand oft bloß gemeinsamem Produktvorkommen |
| R1.1: gewöhnliche Positionsnummer trennt schlechter statt besser; 5.481 K3→K2b, nur 168 K3 verbleiben | N-Bericht; A hat `r1_1_positionstest.py` und die maskierte CSV mit 16.419 × 40 geprüft |
| Sechs vorhandene Alternativpaare besitzen in R1.1 jeweils `pos_overlap = 1` und `co_produkte = 0` | A direkt an der maskierten CSV geprüft; geringe Zahl, kein allgemeiner Rollenbeweis |
| NMF-Eingabe GJL121: 772 Produkte × 103 Komponenten, binäre Matrix | Aus dem ausgeführten Notebooklauf berichtet; 772 Produkte werden durch den Gruppenprofil-Export vollständig abgedeckt |
| Neun NMF-Kandidaten mit 2, 3, 4, 5, 6, 8, 10, 12 und 15 Mustern gerechnet | Aus Notebookausgabe berichtet; Modell mit 10 Mustern erreichte bei 1.000 Iterationen keine Konvergenz |
| Vorläufige Ansicht: Modell mit 6 Mustern; W = 772 × 6, H = 6 × 103 | Form folgt aus NMF und berichteter X-Form; konkrete Rohwerte liegen A nicht als Datei vor |
| `Wahres_ML.csv`: 13 kleinere Gruppen, 772 Produkte, sechs auf Summe ≈1 normierte Gruppenprofilwerte | A selbst geprüft |
| Ähnlichste Paare bei mindestens 8 Produkten je Gruppe: VB6/VB6A 0,9835; VBC6/VBC6A 0,9593; B6/B6S 0,8892 | A aus der auf vier Dezimalstellen gerundeten `Wahres_ML.csv` rekonstruiert; mögliche minimale Rundungsabweichung |
| Sechs Muster sind die optimale Lösung oder die Gruppen sollen automatisch zusammengeführt werden | **Nicht belegt** |

## Was R1 und R1.1 methodisch gezeigt haben

- Reine Eltern-/Nachbarschaftsprofile sind im flachen Export zu unscharf: viele funktional verschiedene Komponenten erhalten fast identische Werte.
- Das Gegenmerkmal „kein normales gemeinsames Auftreten“ reduziert stark, ist aber noch kein positiver Rollenbeweis.
- Normale Positionsnummern unter der Produktwurzel bezeichnen keine stabile funktionale Stelle; sie sind häufig variantenabhängig oder bloße laufende Nummern.
- Der vorhandene `ALT:`-Slot ist ein hilfreicher positiver Anker, weil die Austauschbarkeit bereits gepflegt ist. Er gilt nur, wo diese Information vorhanden ist.
- R1/R1.1 rechtfertigen weder globale Rollengruppen noch eine weitere automatische Strukturregel.

## Was der NMF-Pilot methodisch tut

\[
X_{772\times103}\approx W_{772\times k}\,H_{k\times103}
\]

- X: Welche der 103 Komponenten kommt in welchem der 772 Produkte vor?
- W: Wie stark wird jedes Produkt durch jedes der k gelernten Muster beschrieben?
- H: Welche Komponenten tragen wie stark zu jedem Muster bei?
- Die kleinere Gruppe wird **nicht** zum Training verwendet. Erst nach dem Training werden die relativen W-Profile der Produkte je kleinerer Gruppe gemittelt.
- Gruppenähnlichkeit wird anschließend als Kosinusähnlichkeit zweier durchschnittlicher Musterprofile berechnet.

NMF wurde als Pilot gewählt, weil die Daten nichtnegativ sind und Produkte als additive Mischung mehrerer Muster beschrieben werden können. PCA könnte negative Beiträge liefern; K-Means erzwingt eine harte Gruppenzuordnung; Isolation Forest beantwortet bereits die spätere Ausreißerfrage. Das ist eine fachlich passende Begründung, aber noch kein empirischer Beweis, dass NMF allen Alternativen überlegen ist.

## Bekannte Grenzen und offene Belege

- Der vollständige NMF-Code beziehungsweise das Notebook ist nicht dauerhaft vorhanden.
- Trainingsparameter wie Initialisierung, Solver, Toleranz und Zufallsstart sind im Handoff nicht vollständig belegt.
- `nmf_modelle` und `nmf_ergebnisse` liegen bislang nur im Notebook-Arbeitsspeicher.
- Die rohen W- und H-Matrizen, Produkt-/Komponentenreihenfolgen und H-Interpretation fehlen als Dateien.
- `Wahres_ML.csv` enthält nur gemittelte, zeilenweise normalisierte Gruppenprofile, nicht W selbst.
- Rekonstruktionsfehler sind keine Genauigkeit und dürfen nicht allein die Musteranzahl bestimmen.
- Stabilität über Musteranzahlen, Zufallsstarts und erneute Läufe wurde noch nicht geprüft.
- Sehr kleine Gruppen mit 1–3 Produkten können extreme Ähnlichkeiten erzeugen; sie bleiben sichtbar, werden aber datenarm markiert.
- Mittelwerte können interne Streuung einer kleineren Gruppe verdecken.
- R1.1-Bericht und geprüfte CSV tragen unterschiedliche Lauf-IDs; außerdem ist ein Alternativpaar-Selbsttest im Code wegen `or True` wirkungslos. Die sechs betreffenden CSV-Zeilen erfüllen die Bedingung dennoch tatsächlich.

## Stopppunkt

Noch nicht:

- sechs Muster als Endmodell auswählen;
- kleinere Gruppen automatisch zusammenführen;
- NMF-Ähnlichkeit als Wahrscheinlichkeit bezeichnen;
- aus H Rollen ableiten, bevor H tatsächlich angesehen wurde;
- eine weitere Rolleninformation oder Anomalieerkennung implementieren.

Zuerst den NMF-Lauf reproduzierbar sichern und H sichtbar machen. Danach genau eine Validierungsfrage bearbeiten.

## Zusammenarbeit, Schutz und Praxisarbeit

Ein Schritt pro Auftrag, konkrete Ergebnisse prüfen, verständlich erklären, Grenzen dokumentieren. Keine Nachrichten an N, C oder Fachseite ohne entsprechenden Auftrag. Abgabeziel: **16.10.2026**. Die neue R1/R1.1→NMF-Entwicklung ist für C relevant, wurde C durch diesen Handoff aber nicht automatisch zugesandt.

Konkrete Corporate-Daten bleiben geschützt. Die in diesem Stand verwendeten Namen stammen aus vom Bearbeiter zur Weitergabe freigegebenen Gruppenbezeichnungen; zusätzliche Identnummern und lesbare Materialtexte werden nicht übernommen.