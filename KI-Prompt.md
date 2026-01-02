## 

## 1. Grundstrukur der Fallstudie
*Prompt:* <br>
Schreib mir das ein bisschen schöner:

Folgende Ausgangslage habe ich für meine Fallstudie:

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
1) contract_data.csv — Spalten:
- ID (String, mit PK z.B. CUST0001..CUST1500)
- Branche (kategorisch; z. B. IT, Handel, Finanzen, Öffentlicher Sektor etc.)
- Unternehmensgröße (int, Anzahl Mitarbeiter und realistische Verteilung der Firmen: viele kleine, einige mittlere, wenige große)
- Vertragsbeginn (Datum YYYY-MM-DD; verteilt über die letzten 48 Monate)
- Laufzeit (int; nur 24 oder 36 MonaTe)
- monatliche Grundgebühr (int; korreliert positiv mit Unternehmensgröße und Branche)
2) support_data.csv — Spalten:
- ID (String) -> Die gleichen werte wie bei 1.
-  Anzahl technischer Störungen in den letzten 6 Monaten (int)
- Durchschnittliche Dauuer der Entstörung (float, Stunden)
- Anzahl der Beschwerdeanrufe (int)
3) usage_data.csv — Spalten:
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
