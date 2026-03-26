# ToDo

## Custom Hardware (PCB)

**Entscheidung:** Google Coral SOM (G313-04345-01) als Hauptmodul statt Raspberry Pi

- [ ] Coral SOM Datenblatt + Referenzschaltplan herunterladen (coral.ai/docs/som/datasheet)
- [ ] Schaltplan in KiCad erstellen (Carrier Board)
- [ ] PCB-Layout erstellen (4-lagig, Impedanzkontrolle für MIPI CSI + USB)
- [ ] Gerber-Dateien an JLCPCB / PCBWay senden
- [ ] Bauteile bestellen:
  - Google Coral SOM G313-04345-01 — **~110 €** (Mouser)
  - SO-DIMM 260-pin Buchse TE 2309990-1 — **~5 €**
  - Arducam IMX219 NoIR Kamera (CSI-2) — **~25 €**
  - FPC-Stecker 15-pin Molex 5031480150 — **~1 €**
  - HX711 SOIC-16 (Waagen-ADC) — **~1–2 €**
  - Wägezelle 1kg TAL220B — **~10 €**
  - TPS54331DR SOIC-8 Buck Converter (×2) — **~4 €**
  - USB-C Buchse USB4135-GF-A — **~2 €**
  - CP2102N-A02-GQFN24 USB-UART Bridge — **~4 €**
  - BAT54S SOT-23 Schottky-Diode — **~0,50 €**
  - Kondensatoren, Widerstände, LEDs, Taster (Sortiment) — **~8 €**
  - PCB-Fertigung 4-lagig (JLCPCB, 5 Stück) — **~20 €**
  - **Gesamt: ~190 €**
- [ ] PCB bestücken und löten

## Modell-Konvertierung (PyTorch → Edge TPU)

- [ ] MobileNetV2: PyTorch → ONNX → TFLite (int8 quantisiert) → EdgeTPU compile
- [ ] YOLOv8n: `model.export(format='tflite', int8=True)` → EdgeTPU compile
- [ ] Inferenz auf Coral SOM testen und Genauigkeit prüfen

## Software-Setup (Coral SOM)

- [ ] Mendel Linux / Debian auf Coral SOM aufsetzen
- [ ] PyCoral + TFLite Runtime installieren
- [ ] NoIR-Kamera (Arducam IMX219) per MIPI CSI einrichten
- [ ] HX711 Waagen-Auslese in Python implementieren (SPI/GPIO)
- [ ] Nullstellung der Waage per Taster realisieren
- [ ] Autostart beim Hochfahren konfigurieren

## Hauptprogramm (`main.ipynb`)

- [ ] YOLO + MobileNetV2 + Waage + Datenbank zu einem Gesamtsystem zusammenführen
- [ ] Gewicht beim Erscheinen der Katze erfassen (Start-Event)
- [ ] Gewicht beim Verschwinden der Katze erfassen und Differenz berechnen (End-Event)
- [ ] Fütterungseintrag in PostgreSQL schreiben

## Offline-Pufferung

- [ ] Lokale SQLite-Datenbank als Zwischenspeicher bei fehlender NAS-Verbindung
- [ ] Bei jedem neuen Eintrag Verbindungsversuch zur PostgreSQL starten
- [ ] Gepufferte Einträge bei erfolgreicher Verbindung synchronisieren

## Offene konzeptionelle Fragen

- [ ] Wie werden zwei Fressnäpfe gleichzeitig überwacht? (z.B. zwei Kameras, oder ein großer Teller)
- [ ] Was passiert wenn beide Katzen gleichzeitig am selben Napf fressen?
- [ ] Automatische Erkennung der Futtersorte (Nass-/Trockenfutter, Marke)

## Deployment

- [ ] Robustheit testen (Dauerbetrieb, Lichtbedingungen, Kamerawinkel)
