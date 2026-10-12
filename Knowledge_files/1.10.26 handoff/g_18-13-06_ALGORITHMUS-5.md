# 06 — Algorithmische Entwicklung

## Gültiger Big3-Stand, 01.10.2026

Big3 beantwortet dieselbe Frage wie Big1 (ist exakte Komponente X in dieser Fassung zu erwarten?) mit denselben Lernzeilen, produktgetrennten fünf Folds, Gewichten und CatBoost-Parametern wie Big1 V2, ergänzt um **14 Strukturmerkmale** aus der Cleaner-Ausgabe: übliche Träger-Baugruppe von X vorhanden/aufgelöst/am Erzeugnis; übliche Stelle (SAP-Stufe + Positions- oder ALT-Code) und deren Belegung durch andere Komponenten (Anzahl, Menge, ALPGR, Einsatzwahrscheinlichkeit); Eigenschaften von X im Training (ALPGR-Anteil, EWAHR-Median, Phantom-Herkunft `stufe>tiefe`, Mengenmedian); Anteil und Ähnlichkeit der zehn ähnlichsten Trainingsblöcke anderer Produkte, wobei die Ähnlichkeit **ohne X** berechnet wird. Alle Merkmale werden je Fold nur aus Trainingsblöcken gebildet.

Ergebnis GJL121 [G geprüft]: Log-Loss in allen fünf Folds unter Big1 V2 (Lauf 5: ≈0,029 → ≈0,016; Lauf 3 mit verworfener Excel-Quelle ≈0,011; Lauf 6 Block 1 fehlt). Spule 60 V (bestätigt) 0,996; Joch 0,99 → 0,71 mit Begründung „Träger vorhanden, nicht aufgelöst“; Hilfskontaktbrücke 0,998 → 0,21 über Menge an der Stelle; Gehäuse-Variante P0009 **nicht** gelöst (4 Vorkommen). Von den 100 stärksten Big1-Fehlbefunden 33 stark abgeschwächt, 28 abgeschwächt, 18 leicht, 21 bestätigt; 23 neue Kandidaten. Wichtigkeit: Nachbarmerkmal 36 %, danach Stellenmenge, Phantom-Anteil, Nachbar-Jaccard. **Grenzen:** ein Fachlabel; Nachbar-Dominanz bei Unikaten; Varianten/Ersetzungen (ursprüngliche Big3-Idee) unbearbeitet; kein gespeichertes Modell. Stacking (`v2+big1`) vorbereitet, nicht gelaufen. Verworfen: Excel-Parser als zweite Datenquelle (Universalität), V1 als Basis (Spulenfall), Nachbarbildung auf Joch zugeschnitten (Nutzer). Chronologie, Merkmalstabelle, Kontrollfälle: [G_HANDOUT](G_HANDOUT_Big3_2026-10-01.md) Abschnitte 2–5.

Spätere Optionen [Hypothese]: Stacking-Vergleich; Lauf auf GJL131; Ausgabe `p_big3`/`auff_big3` plus zwei Strukturbegründungen je Zeile für die gemeinsame Liste; Varianten-/Ersetzungsmodell.

## Gültiger Big2-Pilot, 28.09.2026 (Big3-Satz darin abgelöst)

Big1 V2 schätzt Anwesenheit exakter Komponenten, Big2 schätzt bei **vorhandenen** Komponenten deren plausible Mengen im Produktblock; späteres Big3 würde Varianten und Ersetzungen fachlich verwandter Komponenten untersuchen. Die frühe Reihenfolge „erst Komponentengruppen, dann Mengenpilot“ unten wurde durch den tatsächlich ausgeführten Big2-Pilot abgelöst.

Big2 maskiert die geprüfte Menge und Positionszahl, lernt die Zielzustände aus Komponentenkennung, generischem Produkt-/Spannungskontext und den Mengen der anderen Komponenten. Fünf zusätzliche numerische Merkmale fassen bei **zehn** per Jaccard ausgewählten Nachbarblöcken Median/Min/Max der Komponentenmengen, Komponentendeckung und mittlere Ähnlichkeit zusammen. Nur Trainingsfolds derselben Hauptgruppe und nie das eigene Produkt werden als Nachbarn zugelassen. Produktgetrennte fünf Folds, getrennte Modelle für GJL121/GJL131. `P(Menge | Komponente)` aus Trainingsfolds ist eine Vergleichsbaseline, kein zweites Big2-Modell im produktiven Ranking.

Nach der Modellvorhersage werden Zeilenkandidaten zu Produktblöcken aggregiert: Anzahl, Summe `−log p_ist` und konsistentes Verhältnis von Ist- zu erwarteter Menge. Beim fachlich bestätigten halbierten Block ist die Produktaggregation der entscheidende Ranghebel (Rang 1); Nachbarn senken den berichteten Log-Loss in beiden Hauptgruppen um etwa 20 % gegen dasselbe Modell ohne Nachbarn. Vier von vier Handinjektionen wurden berichtet; allgemeine Recall-/Falschalarmwerte stehen aus. Scores sind keine Wahrscheinlichkeit eines wirklichen SAP-Fehlers. Details, Quellenstatus, Kennzahlen und systematischer Prüfplan: [Big2-Handoff](PA1_BIG2_Handoff_2026-09-28.md).

## Historischer Reverse-/Rollen-/NMF-Stand

Historischer Stand vom 09.09.2026: R1/R1.1 und anschließender NMF-Pilot. Dieser Teil bleibt als Versuchsgeschichte erhalten; der aktuelle Anschluss steht im Aktualisierungsblock oben.

## Methodischer Stand der früheren R1/NMF-Phase

### R1/R1.1: Rollenweg vorerst gestoppt

R1 verglich 16.419 Komponentenpaare über gewichtete Eltern-/Nachbarschaftsprofile. 14.299 Paare hatten messbare Ähnlichkeit, aber 14.031 davon besaßen kein gemeinsames internes Elternteil. Die flache Produktwurzel ließ funktional verschiedene Komponenten fast identisch erscheinen. 5.649 Paare blieben als „ähnlich ohne beobachteten Widerspruch“ übrig: zu viel für eine sinnvolle automatische Vorauswahl.

R1.1 testete genau eine Ergänzung: lokale Position. Die gewöhnliche Positionsnummer erwies sich nicht als funktionale Stelle. Von 250 zuvor plausiblen K3-Paaren mit Strukturwert ≥ 0,90 wurden nur 6 gestützt; 244 gingen verloren. Gleichzeitig wurden funktional unplausible Paare häufiger gestützt. Vorhandene `ALT:`-Slots funktionierten dagegen bei allen sechs bekannten Alternativpaaren, enthalten aber bereits gepflegtes Rollenwissen und decken nur einen Teil des Bestands ab.

**Schluss:** Reine abgeleitete Struktur plus Positionsnummer reicht nicht. Keine weitere Rollenstufe ist aktiv. R1/R1.1 bleiben als wichtige negative beziehungsweise teilweise brauchbare Versuche erhalten.

### NMF-Pilot: direkter Vergleich von Produkten und kleineren Gruppen

Für die Hauptgruppe GJL121 wurde eine binäre Matrix gebildet:

\[
X\in\{0,1\}^{772\times103}
\]

Zeile = Produkt, Spalte = Komponente. NMF approximiert:

\[
X\approx W H,
\quad W\ge0,\ H\ge0
\]

Bei k Mustern gilt W `772 × k` und H `k × 103`. W beschreibt die Mustermischung jedes Produkts; H beschreibt die Komponenten jedes Musters. Kleinere Gruppen und ihre Namen gehen nicht in das Training ein. Erst anschließend werden W-Zeilen relativiert, je kleinerer Gruppe gemittelt und über Kosinus verglichen.

Neun Kandidaten wurden berichtet:

| k Muster | relativer Rekonstruktionsfehler |
|---:|---:|
| 2 | 0,4760 |
| 3 | 0,4284 |
| 4 | 0,3934 |
| 5 | 0,3545 |
| 6 | 0,3341 |
| 8 | 0,3035 |
| 10 | 0,2649; nach 1.000 Iterationen noch nicht konvergiert |
| 12 | 0,2453 |
| 15 | 0,2110 |

Der Fehler ist keine Fehlerquote und keine Genauigkeit. Er sinkt erwartbar mit wachsendem k und bestimmt die Musteranzahl nicht allein. Das 6-Muster-Modell ist nur der erste untersuchte Snapshot.

### Erster Gruppenbefund aus dem 6-Muster-Snapshot

`Wahres_ML.csv` enthält 13 gemittelte Gruppenprofile und zusammen 772 Produkte. Aus den auf vier Dezimalstellen gerundeten Profilen rekonstruierte A 78 Kosinuswerte. Bei mindestens 8 Produkten je kleinerer Gruppe liegen oben:

| Paar | Produkte | Ähnlichkeit |
|---|---:|---:|
| VB6 / VB6A | 45 / 38 | 0,9835 |
| VBC6 / VBC6A | 70 / 30 | 0,9593 |
| B6 / B6S | 255 / 8 | 0,8892 |

Das ist bemerkenswert, weil die Gruppennamen nicht trainiert wurden. Es ist trotzdem kein Fachbeweis und keine automatische Zusammenführung. Über alle 78 Paare liegt B6/B6S auf Rang 15; bei Mindestgröße 8 auf Rang 3 von 21. Extrem hohe Gesamtwerte stammen teilweise aus Gruppen mit nur 1–3 Produkten.

### Warum NMF als Pilot und nicht direkt PCA, K-Means oder Isolation Forest?

Das Hauptargument ist nicht Rechenersparnis: 103 Komponenten sind technisch klein. NMF liefert eine **nichtnegative additive Darstellung**, in der ein Produkt mehrere Komponentenmustern gleichzeitig enthalten kann und H die beteiligten Komponenten sichtbar hält.

| Verfahren | Andere Hauptfrage / Grenze im aktuellen Schritt |
|---|---|
| PCA/SVD | erklärt Varianz mit positiven und negativen Beiträgen; weniger natürliche additive Stücklisteninterpretation |
| K-Means | ordnet jedes Produkt hart einem Cluster zu; bildet keine Mustermischung ab |
| Isolation Forest | sucht Ausreißer, bevor belastbare Vergleichsgruppen feststehen |
| Hierarchisches Clustering | bereits als Gruppenvergleich vorhanden; liefert Gruppenbaum, aber keine additive Komponentenzerlegung |
| Direkte Kosinus-/Jaccard-Ähnlichkeit | wichtige Baseline, kann einzelne Varianten hart bestrafen und lernt keine latenten Muster |

NMF ist fachlich passend, aber noch nicht empirisch als beste Methode bewiesen.

### Historischer nächster Prüfschritt des NMF-Piloten (abgelöst)

1. Vollständige Notebook-/Trainingszelle und Parameter sichern.
2. `nmf_ergebnisse`, rohe W/H, Produkt- und Komponentenreihenfolge exportieren.
3. H für einen Kandidaten, zunächst k=6, über die stärksten Komponenten je Muster ansehen.
4. Erst danach genau eine Validierung wählen: Stabilität über k oder über Zufallsstarts.

Nicht gleichzeitig Endmodell, Clustering und Anomalieerkennung bauen. Vollständiger Stand und Exportcode: [08](09-08_NMF_PILOT_STAND_PRUEFPLAN-2-.md).

## Historischer Zwischenstand vor Lieferung von R1 — nicht aktuelle Arbeitsanweisung

Eine bereitgestellte Stückliste enthält Produkte und deren Komponenten. Zunächst werden Produkte nach den ersten sechs Stellen ihrer Identnummer in Hauptgruppen eingeordnet. Hauptgruppen bleiben immer getrennt. Innerhalb einer Hauptgruppe sollen kleinere Gruppen, etwa B6 und B6S, so weit fachlich sinnvoll zusammengefasst werden. Der Name bleibt beispielsweise „B6 und B6S“.

1. **Komponenten:** Können unterschiedliche Komponenten im jeweiligen Aufbau als hinreichend ähnliche Varianten einer Rolle behandelt werden?
2. **Kleinere Gruppen:** Passen ihre Komponenten nach Berücksichtigung solcher Varianten ausreichend zusammen?
3. **Anomalien:** Welche Abweichungen sind im so gewonnenen Vergleich auffällig? Dies wird später separat entwickelt.

Die Rollenidee könnte das methodische Herz werden. Das ist eine Entwicklungsrichtung, kein belegtes Ergebnis. Eine gleiche Komponenten-Identnummer kann in unterschiedlichen Zusammenhängen verschiedene Aufgaben erfüllen. Ein globales Profil je Identnummer kann solche Unterschiede verwischen; R1 muss diese Einschränkung offenlegen.

## R0 und der damalige R1-Versuch

R0 prüfte laut N die verfügbaren Felder und rekonstruierbare Struktur; Zahlen und Grenzen stehen in 03. R1 untersucht zunächst ausschließlich die bereinigte Umgebung einer Komponente: konkrete übergeordnete Komponenten und andere direkte Komponenten unter demselben Elternknoten im selben Block. Die bereinigte `tiefe` wird nur als Kontrollinformation mitgeführt.

Seltene Umgebungsinformationen sollen über eine dokumentierte datenabhängige Gewichtung stärker zählen als fast überall auftretende. Der Auftrag nennt beispielsweise IDF und anschließend Kosinusähnlichkeit. Das ist ein erster struktureller Vergleich, noch kein Beweis erlernter technischer Rollen. Ein Wert von 0,95 ist keine 95-prozentige Wahrscheinlichkeit gleicher Rolle.

Es werden unterschiedliche Komponentenpaare ausschließlich innerhalb derselben Hauptgruppe betrachtet. Texte, kleinere Gruppen, Mengen, Positionen, ursprüngliche Stufe und J-Entscheidungen fließen nicht in den Ähnlichkeitswert ein. `alt_wahrsch` bleibt ausgeschlossen. Blöcke bleiben getrennt, gleiche Produkt-Identnummern verstärken dieselbe Information nicht mehrfach.

Gemeinsames Auftreten ist Gegeninformation, aber keine allgemeine fachliche Ausschlussregel: SAP-Alternativen können gemeinsam aufgeführt sein, ohne gemeinsam eingebaut zu werden. Ein `ALT:`-Slot wird zur Behandlung dieses Falls verwendet und zur nachträglichen Kontrolle, nicht zur Erhöhung des Rohwerts. Eine Produktwurzel allein erzeugt keine Ähnlichkeit. Ihre vielen direkten Kinder können dennoch das Ergebnis dominieren; das ist zu prüfen.

**Die Sorge des Nutzers:** Ein Verfahren, das fast alles mit „Bitte selbst prüfen“ abschließt, entlastet zu wenig. R1 soll die Verteilung und die Größe der verbleibenden Auswahl zeigen. Eine kurze Liste allein beweist keine Qualität; interessante Fälle könnten ausgeschlossen worden sein. Noch keine Automatisierungssicherheit versprechen.

Historisch war der vollständige R1-Auftrag samt Prüfplan maßgeblich; dieses gesonderte Altartefakt ist nicht Teil des aktuellen Pakets. Kein LLM für den damaligen Ansatz, kein fester Wochenkalender. Fachliche Rückmeldung kann per E-Mail erfolgen; das ist keine Erlaubnis für A, ungefragt Nachrichten zu senden.

## Was J und das bestehende Gruppen-Clustering leisten

J ist die fachlich motivierte bisherige Vergleichsmethode. Die Neutralisierung anhand desselben ersten Wortes im Kurztext beweist keine Komponentenpaare und keine Rollen. Bei acht der neun akzeptierten Paarvergleiche hängt die Entscheidung an dieser Heuristik. Die Grenze von höchstens einem verbleibenden Unterschied wurde anhand B6/B6S festgelegt; dieses Beispiel ist danach kein unabhängiger Test genau dieser Grenze.

Das bereits ausgeführte hierarchische Clustering ordnet **kleinere Gruppen als Ganzes** anhand der Häufigkeit konkreter Komponenten. Es betrachtet 23 Gruppenprofile, nicht 1.692 Produkte als einzelne Trainingsbeispiele. A hatte Beobachtungen, Merkmale und Kosinusdistanz vorgegeben; N wählte Average Linkage, implementierte und führte aus. Es lernt keine Gleichwertigkeit verschiedener Komponenten und ist kein Anomaliedetektor. Es kann als bisheriger Vergleich für neue Versuche erhalten bleiben.

A prüfte an der Masterdatei 89 Paarzeilen, 9 ZUSAMMEN, 80 GETRENNT und die daraus berichteten 15 Gruppen. B6/B6S hat dort Kosinusähnlichkeit 0,9495. Das ist Unterstützung durch ein weiteres Verfahren mit gemeinsamem Datenfundament, kein unabhängiger fachlicher Beweis. Alle 89 Expertenfelder bleiben OFFEN; die außerhalb der Tabelle bestätigte Nutzerentscheidung zu B6/B6S ist gesondert zu nennen.

Offen bleiben unter anderem Ns Gruppenzählung bei veränderten Schwellen, Rundung/Ränge und die Regel hinter `ml_vs_j`. Bei maximal 0/1/2/3 verbleibenden Unterschieden ergeben sich 2/9/18/29 akzeptierte Paare. Ns Gruppenzahlen 21/15/13/10 weichen von As Berechnung über Zusammenhangskomponenten 21/15/11/8 ab. Dies klärt die verwendete Methode, es legt keine zukünftige Regel für Ketten aus ähnlichen Paaren fest.

## Später, ausdrücklich noch nicht beauftragt

Weitere Merkmale einzeln testen, beispielsweise Kurztext ohne LLM. Welche Erweiterung sinnvoll ist, folgt aus R1. Eine spätere gelernte Darstellung oder ein neuronales Netz ist eine Option, kein beschlossener Endpunkt. Lineare Regression ist nicht automatisch der passende Start für unüberwachtes Rollenlernen; eine Regression auf fachliche Zielwerte wäre ein anderer Lernauftrag. Bestätigte Beispiele können später zur Bewertung oder zum Lernen dienen; diese Nutzungen sauber auseinanderhalten.

Paarähnlichkeit und eine ganze Rollengruppe sind verschieden: A kann zu B passen, B zu C, A aber nicht zu C. Erst reale Fälle untersuchen, keine universelle Gruppenregel vorab festlegen. Neue Produkte und neue Exporte sind das angestrebte Einsatzgebiet, nicht bereits geprüfte Generalisierung.

## Historische Entwicklung — nicht aktuelle Arbeitsanweisung

Die folgenden Originalabschnitte bewahren Versuche und ihre damaligen Interpretationen. Insbesondere „erst Wissen aufbauen“, zukünftige Master-/ML-Aufträge und damalige Statushinweise sind abgelöst. Frühere Wörter wie „bewiesen“ sind ohne separat belegte fachliche Prüfung nur damalige Interpretation. Aktueller Stand und Freigabe stehen ausschließlich oben und in 04/07.

<details>
<summary>Historischer Wissensstand bis 04.09.2026 (zum Nachschlagen)</summary>

## 06 — PHASE 1 / PATTERN MATCHING / VERGLEICHBARKEIT

> **Statushinweis:** Die Abschnitte 1–8 dokumentieren den Lern- und
> Explorationsstand bis 03.09.2026. Der aktuelle methodische Stand folgt in
> Abschnitt 9.
>
> Dieses File hält bewusst Befunde, Begriffe, Hypothesen und offene Entscheidungen getrennt.

---

### 1. Warum dieses File existiert

Der Clean-Algorithmus liefert jetzt einen strukturierten Datensatz. Der nächste wissenschaftliche Schritt ist **nicht sofort Anomalieerkennung**, sondern:

> **Was darf überhaupt sinnvoll miteinander verglichen werden?**

Der Bearbeiter hat ausdrücklich gesagt, dass er die aktuellen Begriffe und Abstraktionen noch nicht ausreichend versteht. Deshalb gilt:

> **Vor weiterem Code zuerst Wissen aufbauen.**

Der nächste A-Chat soll Parent-/Child-Strukturen, Einheiten, Slots, Vergleichsgruppen, Analyseeinheiten und Pattern Matching von Grund auf erklären, bis der Bearbeiter sie selbst in Meetings erklären und eigenständig zu Methoden recherchieren kann.

---

### 2. Begriffe — NOCH NICHT als fachlich endgültige Definitionen lesen

#### Parent / Child

Der Cleaner rekonstruiert aus der flachen Stücklistenliste eine technische Eltern-Kind-Struktur.

Ein **Parent** ist dabei ein Knoten, unter dem direkte **Children** hängen.

#### Parent-Child-Einheit

Technisch:

```text
Parent
├── direktes Kind A
├── direktes Kind B
└── direktes Kind C
```

Diese Einheit ist eine **rekonstruierte Struktureinheit**.

Wichtig:
- nicht automatisch mit dem fachlichen Begriff „Baugruppe“ gleichsetzen;
- ob und wann diese Einheit fachlich einer Baugruppe entspricht, muss sauber verstanden/ggf. mit Fachseite geklärt werden.

#### Wurzel-Einheit

Kinder, die direkt am Erzeugnis hängen.

#### Slot / Position

Gedanklich: eine Stelle innerhalb einer Struktur, die über verschiedene Vorkommen unterschiedlich besetzt sein kann.

Aktueller Core:
- Alternativgruppe vorhanden → Alternativgruppe bildet gemeinsam einen Slot
- sonst Positionsnummer
- sonst technischer Fallback

#### „Topf“ / Vergleichsgruppe

Arbeitsbegriff:

> Menge von Objekten, die man gegeneinander halten darf, weil sie ausreichend vergleichbar sind.

Noch offen:
- Erzeugnis-Topf?
- Parent-Child-/Baugruppen-Topf?
- hierarchische Kombination?

#### Produktgruppe

**Noch nicht als Zielbegriff festgelegt.**

Frühere Skripte gruppierten Erzeugnisse grob nach Kurztext/Nummernstamm. Ob die neue Phase 1 tatsächlich „Produktgruppen“ bildet oder allgemeiner **Vergleichsgruppen / Cluster / strukturelle Typen**, ist offen und soll erst nach Verständnis entschieden werden.

#### Analyseeinheit

Das Objekt, auf dem ein Verfahren lernt/vergleicht.

Mögliche Ebenen:
- gesamtes Erzeugnis
- Parent-Child-Einheit
- Slot/Position
- Kombination/Hierarchie daraus

---

### 3. Was „Pattern Matching“ hier wahrscheinlich meint

Der Begriff kommt aus dem Fachbereich.

Nicht vorschnell mit Regex/festem Regelmatching gleichsetzen.

Arbeitsinterpretation:

```text
wiederkehrende Struktur erkennen
        ↓
vergleichbare Fälle zusammenhalten
        ↓
Normalzustand / typische Besetzung aus der Masse lernen
        ↓
ungewöhnliche Abweichungen priorisieren
```

Methodisch wird das später eher **Mustererkennung + unüberwachte/unsupervised Analyse + Anomaliepriorisierung** sein.

---

### 4. Vorläufige Messung von N

#### Strukturelle Einheiten

```text
4.870  Parent-Child-Einheiten insgesamt
1.822  Wurzel-Einheiten
3.048  tiefere Einheiten
```

Kinder je Einheit in der Messung:
- min 1
- max 28
- Mittel ~9,7

#### Wiederholung tiefer Parents

```text
46  verschiedene Parent-Materialnummern
44  Parent-identitäten über teil_id
43  Parents mit >=2 Vorkommen
17  davon in der Messung invariant
26  davon in der Messung variabel
15  Parent-IDs mit >=50 Vorkommen
```

Häufigster Parent laut N: 1.539 Vorkommen.

Interpretation, die **getragen** ist:
- es gibt starke Wiederholung;
- es gibt gleichzeitig Varianz;
- damit existiert grundsätzlich Lern-/Vergleichsmaterial.

Nicht getragen:
- dass jede gleiche Materialnummer fachlich immer dieselbe Baugruppe ist;
- dass die zwei beobachteten Formen automatisch legitime Varianten sind.

#### Ebene der Varianz

N meldet, dass die Varianz im bereinigten Baum stark auf der obersten rekonstruierten Ebene konzentriert ist und tiefere Ebenen weitgehend invariant wirken.

Wichtige Kontextinformation:
- die freigegebene Dummy-Auflösung hat sehr viele Positionen nach oben umgehängt;
- dadurch ist der bereinigte Baum bewusst flacher als der ursprüngliche Rohbaum;
- daraus folgt **nicht**, dass die Dummy-Regel falsch ist.

#### Slots

N meldet:

```text
17.402  Slots
 5.846  Slots mit mehr als einer beobachteten Besetzung
```

Vor methodischer Nutzung klären:
- wurde dabei die Core-Definition D9 (Alternativgruppe = gemeinsamer Slot) vollständig benutzt?
- oder wurde vereinfacht `Parent-Materialnummer + Positionsnummer` gezählt?

---

### 5. Wichtige Einwände von A

#### 5.1 Exakt verschieden ≠ unähnlich

Dass viele Wurzel-Zusammensetzungen exakt einzigartig sind, beweist nicht, dass Erzeugnisse als Vergleichsebene ungeeignet sind.

Beispiel:

```text
A = [1,2,3,4,5]
B = [1,2,3,4,6]
```

Exakt verschieden, aber strukturell sehr ähnlich.

#### 5.2 „Zwei Formen“ erklärt die Ursache nicht

Mögliche Gründe:
- legitime Variante
- Gültigkeits-/Zeitstand
- Alternativposition
- Export-/Struktureffekt
- echte Inkonsistenz
- andere bisher unbekannte Ursache

Darum keine Bedeutung hineininterpretieren, bevor ein Beispiel geprüft wurde.

#### 5.3 Gleiche Parent-ID ist zunächst nur ein harter Anker

Gleiche Materialnummer kann für die erste Messung praktisch sein.

Sie ist aber nicht automatisch die endgültige fachliche Definition von „dieselbe Baugruppe“.

#### 5.4 `teil_id` nicht als Wahrheit erben

`teil_id` stammt primär aus normalisiertem Kurztext. Für Phase 1 getrennt betrachten:
- Materialnummer
- `teil_id`
- strukturelle Ähnlichkeit

---

### 6. Mögliche methodische Richtungen — NOCH KEINE AUSWAHL

#### A. Einfache / klassische Ähnlichkeitsmaße

Beispiele:
- Jaccard
- Kosinus auf sparse Vektoren
- Mengen-/Multimengenvergleich
- strukturierte Feature-Distanzen

Nutzen:
- transparent
- guter Baseline-/Explorationsschritt

#### B. Clustering

Möglicher echter unsupervised-ML-Schritt für Vergleichsgruppen.

Nicht automatisch k-Means wählen. Erst Datenrepräsentation und Distanz klären.

Mögliche Familien:
- hierarchisches Clustering
- dichtebasierte Verfahren
- k-Means nur wenn Geometrie/Features dazu passen

#### C. Association Rule / Frequent Itemset Mining

Später interessant für Aussagen wie:

```text
Wenn A und B auftreten, tritt normalerweise auch C auf.
A und X treten fast nie gemeinsam auf.
```

Kennzahlen:
- Support
- Confidence
- Lift

Aber erst sinnvoll, wenn die richtigen Vergleichsgruppen/Analyseeinheiten feststehen.

#### D. Embeddings / Item2Vec / Node2Vec

Später interessant für funktionale Rollen/Substitutionen bzw. Kontextrepräsentation.

Nicht als erstes Rückgrat einsetzen, solange einfachere und besser erklärbare Signale reichen.

#### Präzision zur bisherigen Arbeit

- TF-IDF = Gewichtungs-/Featureverfahren aus Information Retrieval
- Kosinus/Jaccard = Ähnlichkeitsmaße
- Häufigkeitsprofile = Statistik
- diese Dinge nicht nur deshalb pauschal als „Machine Learning“ labeln, weil der PA1-Titel ML enthält

---

### 7. Nächster Schritt — EXPLIZIT

#### Zuerst: Bearbeiter auf Wissensstand bringen

Der nächste Chat soll **noch nichts implementieren**.

Er soll mit Beispielen und Rückfragen erklären, bis der Bearbeiter selbst beantworten kann:

1. Was ist eine Parent-Child-Einheit?
2. Warum ist „Baugruppe“ als Wort gefährlicher als es klingt?
3. Was ist ein Slot und warum ist Alternativgruppe relevant?
4. Was ist eine Vergleichsgruppe / ein „Topf“?
5. Was ist eine Analyseeinheit?
6. Was wäre ein Erzeugnis-Topf vs. ein Parent-Topf?
7. Was ist der Unterschied zwischen „exakt gleich“ und „ähnlich“?
8. Was ist Pattern Matching in diesem Projekt?
9. Wo würde ein Normalzustand gelernt?
10. Wo könnte später ML sinnvoll sein?

#### Danach

Erst wenn der Bearbeiter sagt, dass das Verständnis sitzt:

1. **einen** variablen Parent exemplarisch auseinandernehmen;
2. Form A / Form B und ihre Slots zeigen;
3. prüfen, welche Felder/Umstände mit dem Unterschied zusammenhängen;
4. danach Methoden recherchieren und Phase 1 auswählen;
5. erst nach Go implementieren.

---

### 8. Forschungs-/Lernfragen für den Bearbeiter

Nach der Grundlagenrunde kann der Bearbeiter eigenständig recherchieren:

- Wie repräsentiert man Stücklisten/BOMs als Vektoren oder Mengen?
- Welche Ähnlichkeitsmaße eignen sich für sparse binäre/Count-Daten?
- Was unterscheidet hierarchisches Clustering, DBSCAN/HDBSCAN und k-Means?
- Wie misst/validiert man Clustering ohne Ground-Truth-Labels?
- Was sind Frequent Itemsets und Association Rules?
- Was bedeuten Support, Confidence und Lift?
- Wie können Graph-/Item-Embeddings aus Co-Occurrence bzw. Verbaukontext gelernt werden?
- Wie trennt man legitime Varianten von seltenen Anomalien?

---

### 9. Aktueller methodischer Pfad ab 04.09.2026

#### 9.1 Die Vergleichsebene ist jetzt praktisch festgelegt

Für den betrachteten Produktbereich wird der vom Fachbereich vorgeschlagene
**J-Ansatz** als aktuelle Baseline umgesetzt:

1. **Hauptgruppe:** erste sechs Zeichen der Erzeugnis-Identnummer
2. **Originaltyp:** Typangabe aus dem Erzeugnis-Kurztext innerhalb der
   Hauptgruppe
3. **Typvergleich:** vollständige bereinigte Stücklisten über
   Komponenten-Materialnummern vergleichen
4. **Vergleichsgruppe:** Originaltypen zusammenführen, wenn sie fachlich bzw.
   datenbasiert hinreichend vergleichbar sind

Die sechs Zeichen sind eine fachlich gestützte Regel für diesen Bereich und
bleiben konfigurierbar. Sie sind keine universelle SAP-Regel und gehören nicht
in `clean_bom()`.

#### 9.2 Wichtige Klarstellung zum fachlichen Beispiel

Ein bekanntes Basis-/S-Typpaar gehört laut Fachbereich trotz verschiedener
Bezeichnung in dieselbe Vergleichsgruppe. Die Aussage „20 gleiche Komponenten“
war nur ein Beispiel zur Erklärung. Sie ist weder ein gemessener Datensatzwert
noch eine notwendige Zusammenlegebedingung.

#### 9.3 Aktueller N-Prototyp — vorläufige Baseline

Von N auf den echten Daten berichtet:

- 1.692 Erzeugnisse
- vier Hauptgruppen mit 772 / 512 / 407 / 1 Erzeugnissen
- 23 auswertbare Originaltypen
- daraus 15 Vergleichsgruppen, davon 7 mit mehreren Originaltypen
- ein Zusammenlegen ist fachlich vorgegeben, weitere sind datengetrieben und
  müssen vom Fachbereich geprüft werden

Die aktuelle Heuristik arbeitet mit regelmäßig bzw. praktisch nie auftretenden
Komponenten, Gegenstücken derselben Teileart und maximal einem „echten
Trennteil“. Vorläufige Schwellen: regelmäßig ≥ 50 %, praktisch nie ≤ 10 %.
Die Teileart wird derzeit aus dem ersten Wort des Kurztexts abgeleitet. Diese
Annahmen sind **vorläufig**, nicht fachlich bewiesen.

#### 9.4 Was A aus der maskierten Paar-Tabelle schließen konnte

Nur maskierter Stand; N muss jeden Punkt auf den echten Daten bestätigen:

- 89 Paarzeilen sind für die drei auswertbaren Hauptgruppen vollständig.
- Rechenidentitäten der Trennteilzählung sind konsistent.
- Die Paaransicht umfasst 1.656 Erzeugnisse; 36 der insgesamt 1.692 sind darin
  noch nicht erklärt.
- Bei maximal 0/1/2/3 echten Trennteilen werden 2/9/18/29 Paarbeziehungen
  akzeptiert. Bei 1 liegt daher keine offensichtliche natürliche Lücke.
- Die kleinere Seite jedes akzeptierten Paars umfasst nur 3–27 Erzeugnisse,
  Median 6. Prozentwerte müssen deshalb stets mit n/N gezeigt werden.
- Ein berichteter Überlappungswert des fachlichen Referenzpaars ist in der
  maskierten Tabelle bei der 50-%-Kernsicht 0,9091 statt 1,0.
- Einige abgelehnte Paare besitzen höhere globale Ähnlichkeit als akzeptierte;
  die Gegenstück-/Trennteillogik ist also entscheidend und muss sichtbar sein.

#### 9.5 Zielarchitektur der nächsten Iteration

| Schicht | Aufgabe | Darf nicht enthalten |
|---|---|---|
| **Clean** | technische Rekonstruktion und freigegebene Filter | J-/ML-Gruppierungsregeln |
| **Profile** | Präsenz n/N jeder Komponente pro Originaltyp und Hauptgruppe | endgültige Gruppenzuordnung |
| **J-Baseline** | fachlich erklärbare Typzusammenlegung | versteckte automatische Wahrheit |
| **Evidence/Master** | eine zentrale, auditierbare Paar- und Entscheidungsquelle | widersprüchliche Parallelberechnungen |
| **Reporting** | CSV/Excel/HTML aus derselben Masterquelle | eigene Neuberechnung |
| **ML-Vergleich** | unabhängiger zweiter Gruppierungsvorschlag | Überschreiben der J-Baseline |

Die zentrale Master-Paar-Tabelle muss die Entscheidung bis zu konkreten
Komponenten zurückverfolgbar machen: Paar-ID, Hauptgruppe, Typen, Größen, n/N,
Ähnlichkeitsmaße, konkrete Gegenstücke, echte Trennteile, Entscheidung,
Entscheidungsquelle, finale Gruppe, Fachurteil, Parameter-/Codeversion,
Abdeckung und Ausschlussgrund.

#### 9.6 Rolle von Machine Learning

ML kann **jetzt** als Vergleich zur Gruppierung eingesetzt werden, z. B. durch
hierarchisches/agglomeratives Clustering auf Typ-Komponenten-Häufigkeitsvektoren
mit Distanzmatrix und Dendrogramm. Es ist noch kein validierter
Fehlerdetektor, weil bestätigte Fehlerlabels fehlen.

Wichtige Auswertungsregel: Die 89 Paare sind nicht 89 unabhängige Beobachtungen,
sondern Kombinationen derselben 23 Typen. Daher kein zufälliger Pair-Train/Test-
Split. Der Vergleich lautet zunächst: **J-Gruppierung vs. ML-Struktur vs.
Fachurteil**.

#### 9.7 Reihenfolge

1. Master-Tabelle herstellen und maskierte A-Befunde intern validieren.
2. Datengetriebene Zusammenlegungen fachlich bewerten lassen.
3. ML-Vergleich ergänzen, ohne J zu ersetzen.
4. Vergleichsgruppen stabilisieren.
5. Erst danach Komponentenabweichungen als mögliche Fehler priorisieren.

</details>