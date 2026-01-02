# Fallstudie: Churn-Scoring System für B2B-Glasfaser Kunden

## A. Projektübersicht
Entwicklung eines datengetriebenen Scoring-Systems zur Identifikation von Kunden mit hohem Churn-Risiko im B2B-Glasfasergeschäft. Das System berechnet für jeden Kunden einen risikobasierten Churn-Score (0-100) auf Basis technischer, nutzungsbezogener und vertraglicher Kennzahlen.

## B. Zielsetzung
- Frühzeitige Erkennung von Kunden mit erhöhtem Abwanderungsrisiko
- Priorisierung von Kundeninterventionen durch das Customer Success Team
- Reduktion der Kundenabwanderung durch proaktive Maßnahmen

## C. Methodik
### 1) Synthestische Datenbasis
- Vertragsdaten: Branche, Unternehmensgröße, Laufzeit, Grundgebühr
- Supportdaten: Technische Störungen, Entstörungsdauer, Beschwerdeanrufe
- Nutzungsdaten: Bandbreitennutzung, Peaks

### 2) Feature Engineering
- Verbleibende Vertragslaufzeit
- Störungen pro 100 Mitarbeiter
- Unterauslastungs-Flag (<30% Auslastung)
- Kosten pro Mitarbeiter
- Peak-Verhältnis (normiert)

### 3) Scoring-Logik
- Gewichteter Score basierend auf vier Hauptkomponenten:
- Störungen (35%): Häufung in letzten 2 Monaten, Störungen pro Peak
- Vertragsende (30%): Verbleibende Laufzeit (<6 Monate kritisch)
- Nutzung (20%): Unterauslastung der Leitung
- Kundenzufriedenheit (15%): Anzahl Beschwerdeanrufe
