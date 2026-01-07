# KI-Prompt-Dokumentation

## 1. Grundstrukur der Fallstudie
*Prompt:* <br>
Im Folgende findest du die Ausgangslage für meine Fallstudie:

"
Case Study: Churn Scoring (ca. 3h Vorbereitungszeit)
Im B2B-Glasfasergeschäft ist die Bindung von Bestandskunden entscheidend. Ein verlorener Firmenkunde bedeutet nicht nur Umsatzverlust, sondern oft auch den Verlust einer strategisch wichtigen Adresse. Dein Ziel ist es, ein automatisiertes System zu entwickeln, das Kunden identifiziert, die kurz davor stehen, ihren Vertrag nicht zu verlängern oder vorzeitig zu kündigen.
1. Datensynthese (AI-Assisted)
Da wir in diesem Schritt keine echten Kundendaten nutzen, erstellst du dein Test-Set selbst. Nutze einen KI-Co-Pilot(Chat-Bot) deiner Wahl, um  drei verknüpfte CSV-Dateien zu generieren:
•	contract_data.csv: ID, Branche, Unternehmensgröße, Vertragsbeginn, Laufzeit (24/36 Monate), monatliche Grundgebühr.
•	support_data.csv: ID, Anzahl technischer Störungen (letzte 6 Monate), durchschnittliche Dauer der Entstörung, Anzahl der Beschwerdeanrufe.
•	usage_data.csv: ID, durchschnittliche Bandbreitennutzung (in %), Anzahl der "Peaks" (Vollauslastung der Leitung).
2. Entwicklung des Churn-Scores (Python)
Lade die Daten in ein Python-Environment (z. B. Jupyter Notebook oder VS Code) und entwickle einen Churn-Score (0 bis 100) für jeden Kunden.
•	Feature Engineering: Berechne neue Kennzahlen, z. B. die verbleibende Vertragslaufzeit oder das Verhältnis von Störungen zur Unternehmensgröße.
•	Scoring-Logik: Erstelle einen gewichteten Score. Berücksichtige dabei Faktoren wie:
o	Häufung von Störungen in den letzten 2 Monaten.
o	Vertragsende in weniger als 6 Monaten.
o	Starke Unterauslastung der Leitung (Gefahr des "Downsizing" oder Anbieterwechsels).
•	KI-Nutzung: Du darfst die KI explizit nutzen, um Code-Bausteine für die Berechnung oder für statistische Auswertungen zu generieren.
3. Visualisierung & Handlungsempfehlung
Erstelle eine kurze Auswertung deiner Ergebnisse:
•	Welche 5 Kunden haben den höchsten Churn-Score?
•	Visualisiere die Verteilung der Churn-Risiken über die verschiedenen Branchen hinweg.
•	Welche zwei Maßnahmen sollte das Customer Success Team ergreifen, um die Kunden mit dem höchsten Risiko zu halten?
 
Bitte bringe folgende "Unterlagen" zum Teams-Gespräch (virtuell) mit:
1.	Das Python-Skript / Notebook, das sowohl die Datengenerierung als auch die Analyse enthält.
2.	Eine kurze Dokumentation deiner Prompts: Welche Anweisungen hast du der KI gegeben, um zu deinem Ziel zu kommen?
3.	Ein kurzes Fazit: Wo hat die KI geholfen, und wo musstest du manuell eingreifen, weil der KI-Vorschlag unlogisch war?
"

Ganz allgemein erstmal: Wie würdest du bei der Bearbeitung des Projektes am besten vorgehen, um es zu bewältigen? Ich hätte gerne von dir einen groben Überblick, sodass ich die Schwerpunkte für die drei Stunden am besten einschätzen kann.

## 2. Synthetische Datengenerierung
*Prompt:* <br>
Kontext:
Erzeuge synthetische, aber realistische Testdaten für eine Fallstudie zum Churn‑Scoring im B2B‑Glasfasergeschäft. Die Daten dienen ausschließlich zur Analyse und sind echten Kundendaten.

Aufgabe:
Erzeuge drei verknüpfte CSV‑Dateien mit jeweils 1500 Zeilen (IDs CUST0001..CUST1500), UTF‑8 kodiert. Erzeuge nur die exakt aufgeführten Spalten (keine zusätzlichen Felder).
1) contract_data.csv  Spalten:
- ID (String, mit PK z.B. CUST0001..CUST1500)
- Branche (kategorisch; z. B. IT, Handel, Finanzen, Öffentlicher Sektor etc.)
- Unternehmensgröße (int, Anzahl Mitarbeiter und realistische Verteilung der Firmen: viele kleine, einige mittlere, wenige große)
- Vertragsbeginn (Datum YYYY-MM-DD; verteilt über die letzten 48 Monate)
- Laufzeit (int; nur 24 oder 36 MonaTe)
- monatliche Grundgebühr (int; korreliert positiv mit Unternehmensgröße und Branche)
2) support_data.csv  Spalten:
- ID (String) -> Die gleichen werte wie bei 1.
-  Anzahl technischer Störungen in den letzten 6 Monaten (int)
- Durchschnittliche Dauuer der Entstörung (float, Stunden)
- Anzahl der Beschwerdeanrufe (int)
3) usage_data.csv  Spalten:
- ID (String) Die gleichen werte wie bei 1.
- durchschnittliche_Bandbreitennutzung_pct (0–100, float)
- Anzahl der Peaks (int)

Wichtige Vorgaben:
- Konsistenz: IDs grundsätzlich in allen drei Dateien; bis zu 5% der IDs dürfen in support/usage fehlen (simuliere fehlende Messungen).
- Korrelationen: Erzeuge plausible Zusammenhänge (z. B. größere Unternehmen -> höhere Grundgebühr; mehr Störungen -> mehr Beschwerdeanrufe; niedrigere Nutzung häufiger bei kleinen Firmen).
- Reproduzierbarkeit: Verwende einen festen Zufallsseed und gib ihn an.

Output (erwartet):
- Die drei CSVs: contract_data.csv, support_data.csv, usage_data.csv.
- Am Ende eine kurze Statistik (head + describe + fehlende Werte pro Datei).

Wenn du von dieser Spezifikation abweichen musst, melde die Abweichung kurz und begründe sie.

## 3. Analyse
*Prompt*: <br>
Im nächsten Schritt erfolgt die Analyse: Hier erzeugen wir aus den drei CSV-Dateien einige leicht verständliche Kennzahlen. Dazu zählen die verbleibenden Monate bis Vertragsende, Störungsraten relativ zur Firmengröße, ein kurzer Indikator für Störungen in den letzten 2 Monaten, ein Unterauslastungs-Flag und einfache Verhältnisse wie Peaks vs. Durchschnittsnutzung, Störungen pro Peak und wirtschaftliche Kennzahlen wie Kosten pro Mitarbeiter. Zu jeder neuen Spalte notieren wir kurz, warum sie nützlich ist, und schauen uns ein Beispiel an.
Die 2-Monats-Zahl ist nicht direkt vorliegend, daher erklären wir die Annahme offen: Standardmäßig nehmen wir 2/6 der 6-Monats-Störungen als Proxy. Um zu prüfen, wie sensibel das ist, rechnen wir das Signal zusätzlich mit einigen alternativen Faktoren und vergleichen jeweils die Top 5-Kunden. Daraus ergibt sich ein Overlap-Prozent, das zeigt, ob die Prioritäten stabil sind oder ob bestimmte Kunden nur wegen der Annahme hochrutschen.
Anschließend normieren wir alle Kennzahlen auf eine einheitliche Skala (0–100), fassen verwandte Signale zu vier verständlichen Komponenten zusammen (Störungen, Vertragsende, Nutzung, Kundenzufriedenheit) und berechnen daraus einen Churn-Score als gewichtete Summe. Die Gewichte sind konfigurierbar. Vor der Auswertung prüfen wir, dass der Score zwischen 0 und 100 liegt und zeigen die Verteilung.
Zum Schluss prüfen wir die Robustheit: Ein Histogramm visualisiert die Score-Verteilung und wir testen mindestens ein alternatives Gewichtsszenario. Wir vergleichen Top-Listen, berechnen die Rangkorrelation und fassen kurz zusammen, ob die Priorisierung stabil ist oder ob bestimmte Kunden als "unsicher" markiert werden sollten. Alle Annahmen (Proxy-Faktor, Unterauslastungs-Schwelle, Gewichte) werden klar dokumentiert.

*Folgepromt:*
Danke - Hier aber noch einige Anmerkungen, weil du Kennzahlen nicht berechnen sollst, diese aber an als eigene Spalten im df berechnen sollst. Hintergrund ist, dass ich später mit diesen weiterrechnen muss, um den Churn-Score gewichtet ermitteln kann:
Bitte implementiere das Feature‑Engineering:

- Lade die drei CSVs und merge sie per ID.
- Berechne und füge als neue Spalten hinzu: verbleibende_Monate, Stoerungen_pro_100_MA, Stoerungen_letzte_2M (Proxy 2/6), Unterauslastung_Flag, Peak_Verhaeltnis, Stoerungen_pro_Peak, kosten_pro_mitarbeiter, kosteneffizienz.
- Füge zu jeder neuen Spalte eine einzeilige Kommentar‑Erklärung, warum die Kennzahl nützlich ist.
- Gib mir die head() als display() zwischendurch immer als Check aus

*Folgepromt*:
Erstelle mir bitte zusätzlich eine Grafik, die die Verteilung des Churn Scores anhand der Anzahl der Kunden in einem Histogramm darstellt. Verwende dafür die zuvor definierten 1&1-Primärfarbe sowie die Highlight-Farbe.

*Folgepromt*:
Danke dir. Lass uns bitte auch stärker auf die Branchenanalyse eingehen, da ich mit diesem Teil aktuell noch nicht zufrieden bin. Ziel ist es, deutlich mehr statistische Einblicke in die einzelnen Branchen in Bezug auf den Churn Score zu erhalten.

Konkret wünsche ich mir folgende Erweiterungen:
1) Tabelle (Display): Eine Übersicht aller Branchen mit zentralen statistischen Kennzahlen zum Churn Score, z. B. Mittelwert, Median, Standardabweichung, Minimum und Maximum.
2) Tabelle (Display): Eine Darstellung der Risikoverteilung je Branche auf Basis der zuvor definierten Churn-Kategorien (z. B. niedrig, mittel, hoch). Hier sollte ersichtlich sein, wie sich die Kunden pro Branche auf die einzelnen Risikoklassen verteilen.
3) Grafik: Ein gestapeltes Balkendiagramm, das die Risikoverteilung je Branche visualisiert. Jeder Balken repräsentiert eine Branche und ist anteilig nach den definierten Risikokategorien aufgeteilt, sodass Unterschiede zwischen den Branchen auf einen Blick erkennbar sind.

## 4. Visualiserung
*Prompt*: <br>
Nun möchte ich unsere Erkenntnisse aus der Analyse gezielt visualisieren. Ich stelle mir dafür ein kompaktes Dashboard in Form einer 2×3-Grafik vor, das die zentralen Erkenntnisse zum Churn-Verhalten übersichtlich zusammenfasst. Begib dich IN die Lage eines Top Data Scientists - Welche Visuals würdest du hierfür erstellen?

*Folgepromt*: <br>
Okay, dann baue mir dafür ein kompaktes Dashboard in Form einer 2×3-Grafik auf, das die zentralen Ergebnisse der Churn-Risiko-Analyse für B2B-Glasfaser-Kunden zusammenfasst.

Die enthaltenen Visualisierungen und ihre jeweiligen Erkenntnisse sind wie folgt zu interpretieren:
1. Verteilung der Churn-Scores (Histogramm)
Das Histogramm zeigt die Verteilung der Churn Scores über alle Kunden hinweg mit durchschnittliche Churn Score als markierte Linie. Benutze für die Balken bitte die Primärfarbe und den Strich bitte in unserer Highlight-Farbe

2. Verteilung nach Risikokategorie (Balkendiagramm)
Diese Grafik stellt die Anzahl der Kunden je definierter Risikokategorie dar. Es soll in unsere 4 Risikokategorien kategorisiert werden, wobei jede dieser Risikokategorien einen Balken ausmacht. Nimm dafür wieder die unsere defininierten Farben für success, risiko etc.

3. Horizontales Balkendiagramm – Durchschnittlicher Churn Score nach Branche
– Y-Achse: Branchen
– X-Achse: durchschnittlicher Churn Score
– Sortierung nach Score-Höhe

4. Scatterplot – Churn Score vs. Unternehmensgröße
– X-Achse: Unternehmensgröße
– Y-Achse: Churn Score
– Farbskala basierend auf monatlichem Umsatz
– Transparente Punkte zur besseren Überlagerung

5. Scatterplot – Churn Score vs. verbleibende Vertragslaufzeit
– X-Achse: verbleibende Monate
– Y-Achse: Churn Score
– Farbskala entsprechend der Anzahl von Störungen

6. Scatterplot – Churn Score vs. Bandbreitennutzung
– X-Achse: Bandbreitennutzung in Prozent
– Y-Achse: Churn Score
– Farbige Kodierung nach Risikokategorie (Niedrig, Mittel, Hoch, Kritisch)

Achte auf ein einheitliches Layout, klare Achsenbeschriftungen, gut lesbare Titel sowie eine kompakte, managementtaugliche Darstellung aller Visualisierungen.

Insgesamt bietet das Dashboard einen konsistenten Überblick über
(a) die Verteilung des Churn-Risikos,
(b) branchenspezifische Unterschiede sowie
(c) zentrale operative und vertragliche Einflussfaktoren
und bildet damit eine fundierte Grundlage für gezielte Retention-Maßnahmen.

## 5. Handlungsmaßnahmen
*Prompt*: <br>
Nun möchte ich den nächsten Schritt in Richtung konkreter Handlungsmaßnahmen gehen.
Identifiziere zunächst die 5 Kunden mit dem höchsten Churn Score und gib diese strukturiert aus.
Für jeden dieser Kunden sollen zusätzlich folgende Informationen angezeigt werden:
– Churn Score
– Zugeordnete Risikokategorie (basierend auf den zuvor definierten Schwellenwerten)
– Monatlicher Umsatz
– Verbleibende Vertragslaufzeit (in Monaten)
– Hauptrisikofaktoren, die maßgeblich zum erhöhten Churn Score beitragen

Die Hauptrisikofaktoren sollen dabei auf zuvor festzulegenden Kriterien basieren (z. B. kurze Restlaufzeit, hohe Anzahl an Störungen, geringe Bandbreitennutzung, niedrige Nutzungshäufigkeit). Lege transparent dar, welche Faktoren als Risikotreiber definiert wurden und welche davon je Kunde zutreffen.
Stelle die Ergebnisse übersichtlich in Tabellenform dar, sodass die Liste direkt als Grundlage für priorisierte Retention- und Vertriebsmaßnahmen genutzt werden kann.

*Folgeprompt*: <br>
Welche zentralen Handlungsmaßnahmen würdest du für Kunden mit erhöhtem Churn-Risiko empfehlen, um eine Abwanderung proaktiv zu verhindern?

Begründe jede Maßnahme datenbasiert anhand typischer Churn-Treiber (z. B. kurze Restlaufzeit, Serviceprobleme, geringe Nutzung) und beschreibe kurz,
– welches Ziel die Maßnahme verfolgt,
– für welche Risikokunden sie besonders geeignet ist und
– welchen erwarteten Einfluss sie auf die Reduktion des Churn-Risikos hat.
Die Maßnahmen sollen praxisnah, skalierbar und operativ umsetzbar sein und sich direkt aus den zuvor analysierten Churn-Erkenntnissen ableiten lassen
