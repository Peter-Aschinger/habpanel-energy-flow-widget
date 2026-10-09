# [HABPanel] Energiefluss-Widget – PV, Akku, Netz, Haus (Smartphone, Hochformat)

Ein Custom-Widget für HABPanel, das die aktuellen Energieflüsse einer PV-Anlage mit Batteriespeicher zeigt. Es ist für Smartphones im Hochformat optimiert (entwickelt und getestet auf einem Pixel 9).

![Screenshot](screenshots/pixel9.png)

🇬🇧 [English version](README.md)

## Funktionen

- **Live-Flüsse** zwischen PV, Haus, Akku und Netz mit animierten Pfeilen. Das Tempo der Pfeile richtet sich nach der Leistung (3 Stufen mit Hysterese). Die Animation läuft über die Grafikkarte und bleibt auch bei Item-Updates flüssig.
- **Kacheln füllen sich von unten nach oben** (wie bei Victron), mit Ampel-Farbverlauf:
  - PV: nach aktueller Leistung
  - Akku: nach Ladestand (SoC)
  - Netz: nach Bezugsleistung (Einspeisung in Türkis)
- **24-h-Verlauf** in der PV- und der Netz-Kachel, geholt vom openHAB-Chart-Dienst (rrd4j), Achsen ausgeblendet
- **Akku-Kachel:** SoC-Ring mit Markierung für den Mindest-SoC, Restkapazität (kWh) mit Ampel, **+/- Knöpfe zum Ändern des Mindest-SoC** direkt im Widget
- **Monats- und Jahreskachel:** Verbrauch, PV-Ertrag, Netzbezug, Einspeisung, PV-Anteil am Verbrauch mit Fortschrittsbalken (beide Kacheln ausblendbar)
- **Alle Items über die Widget-Einstellungen wählbar**, im Code müssen keine Item-Namen angepasst werden
- **Beschriftungen auf Deutsch oder Englisch** umschaltbar

## Voraussetzungen

- openHAB mit HABPanel (getestet mit openHAB 2.4)
- rrd4j-Persistenz für die Items PV-Leistung und Netzleistung (für die Verläufe)
- Ein aktueller Browser (Chrome bzw. Android WebView ab Version 105). Die Pfeil-Animation nutzt CSS-Container-Einheiten (`cqw`).

## Installation

1. Die Widget-Datei aus dem Ordner [`widget`](widget) herunterladen:
   - [`energiefluss-widget_de.json`](widget/energiefluss-widget_de.json) – deutsche Einstellungen und Beschriftungen
   - [`energy-flow-widget_en.json`](widget/energy-flow-widget_en.json) – englische Einstellungen und Beschriftungen
2. HABPanel → Einstellungen → **Benutzerdefinierte Widgets** → *Widget importieren* und die Datei auswählen
   (alternativ ein neues Widget anlegen und [`template/energy-flow-widget.html`](template/energy-flow-widget.html) einfügen; die Einstellungen müssen dann von Hand angelegt werden).
3. Das Widget einem Dashboard hinzufügen und seine Einstellungen öffnen.
4. Im Tab **Einstellungen** die eigenen Items auswählen (siehe Tabelle unten).

## Einstellungen

### Items

| ID | Beschreibung |
|---|---|
| `pvPower` | Aktuelle PV-Leistung (W) |
| `pvYieldToday` | PV-Ertrag heute (Wh) |
| `pvYieldYesterday` | PV-Ertrag gestern (kWh) |
| `pvYieldMonth` / `pvYieldMonthLast` | PV-Ertrag dieser / letzter Monat (kWh) |
| `pvYieldYear` / `pvYieldYearLast` | PV-Ertrag dieses / letztes Jahr (kWh) |
| `pvShareMonth` / `pvShareYear` | PV-Anteil am Verbrauch, Monat / Jahr (%) |
| `gridPower` | Netzleistung (W), positiv = Bezug, negativ = Einspeisung |
| `gridImportToday` / `gridImportYesterday` | Netzbezug heute / gestern (kWh) |
| `gridImportMonth` / `gridImportMonthLast` | Netzbezug dieser / letzter Monat (kWh) |
| `gridImportYear` / `gridImportYearLast` | Netzbezug dieses / letztes Jahr (kWh) |
| `gridExportYear` / `gridExportYearLast` | Einspeisung dieses / letztes Jahr (kWh) |
| `loadToday` / `loadYesterday` | Gesamtverbrauch heute / gestern (kWh) |
| `loadMonth` / `loadMonthLast` | Gesamtverbrauch dieser / letzter Monat (kWh) |
| `loadYear` / `loadYearLast` | Gesamtverbrauch dieses / letztes Jahr (kWh) |
| `battPower` | Akku-Leistung (W), positiv = Laden, negativ = Entladen |
| `battSoc` | Akku-Ladestand (%) |
| `battEnergy` | Restkapazität des Akkus (kWh) |
| `minSocLimit` | Mindest-SoC (%), wird von den +/- Knöpfen geschrieben |

### Zahlen

| ID | Standard | Beschreibung |
|---|---|---|
| `pvMaxW` | 1500 | PV-Leistung, bei der die PV-Kachel ganz gefüllt ist (Spitzenleistung des Wechselrichters) |
| `gridMaxW` | 3000 | Netzbezug, bei dem die Netz-Kachel ganz gefüllt ist |
| `battCapacity` | 7.5 | Nutzbare Akku-Kapazität (kWh) |
| `socStep` | 5 | Schrittweite der +/- Knöpfe (%) |
| `socMinAllowed` | 5 | Kleinster Mindest-SoC, den die Knöpfe zulassen (%) |
| `socMaxAllowed` | 95 | Größter Mindest-SoC, den die Knöpfe zulassen (%) |

### Schalter und Text

| ID | Typ | Beschreibung |
|---|---|---|
| `hideMonth` | Checkbox | Monatskachel ausblenden (Jahreskachel rückt nach oben) |
| `hideYear` | Checkbox | Jahreskachel ausblenden |
| `hideChart` | Checkbox | PV-Verlauf ausblenden |
| `hideGridChart` | Checkbox | Netz-Verlauf ausblenden |
| `chartPeriod` | Text | Zeitraum der Verläufe: `h`, `4h`, `8h`, `12h`, `D` (Standard, 24 h), `3D`, `W` |
| `gridChartPeriod` | Text | Zeitraum des Netz-Verlaufs, leer = wie `chartPeriod` |
| `language` | Text | `en` = englische Beschriftungen, sonst Deutsch (Standard) |
| `scrollInside` | Checkbox | Widget scrollt in sich selbst. Nur nötig, wenn die Kachel kleiner ist als das Widget; sonst die Kachel hoch genug ziehen und aus lassen |

## Hinweise und Grenzen

- **Verläufe:** Das Widget lädt das Bild von `/chart?...` und schneidet die Achsen ab. Der Ausschnitt ist auf das Standard-Layout des Chart-Dienstes abgestimmt. Bei stark abweichenden Wertebereichen (z. B. lange Achsenbeschriftung wie `-1.000`) kann sich die Kurve am linken Rand leicht verschieben.
- **Skalierung der Verläufe:** Der Chart-Dienst skaliert automatisch auf den höchsten Wert. Kurze Spitzen drücken dadurch den Rest der Kurve zusammen.
- **Alle Items müssen gesetzt sein:** Im Template stehen keine Item-Namen. Fehlt eine Einstellung, bleibt die Stelle leer.
- **Items mit Einheiten** (z. B. `Number:Power` mit „257 W“) werden unterstützt. Die Einheit wird abgetrennt, W/kW und Wh/kWh/MWh werden automatisch umgerechnet. Die +/- Knöpfe senden den Mindest-SoC mit der Einheit des Items (z. B. „40 %“).
- **Flow-Pfeile** sind an die Positionen der Kacheln PV, Haus, Akku und Netz gebunden. Diese Kacheln lassen sich deshalb nicht ausblenden.

## Änderungen

- **v3.4** – Keine Verzögerung beim Scrollen; neue Einstellung `scrollInside`
- **v3.3** – Flüssigeres Scrollen auf dem Smartphone
- **v3.2** – Zahlen-Einstellungen zeigen ihre Standardwerte im HABPanel-Einstellungsdialog
- **v3.1** – Unterstützung für Items mit Einheiten (openHAB 3 und neuer)
- **v3.0** – Erste öffentliche Version: alle Items über Einstellungen, Beschriftungen Deutsch/Englisch, Verläufe für PV und Netz, ausblendbare Kacheln, Füllung im Victron-Stil

Über Rückmeldungen und Verbesserungsvorschläge freue ich mich, gerne als Issue.

## Lizenz

MIT, siehe [LICENSE](LICENSE).
