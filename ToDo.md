# ToDo

## Custom Hardware (PCB)

**Entscheidung:** Radxa CM3 (Rockchip RK3566) als Hauptmodul — läuft PyTorch direkt, keine Modell-Konvertierung nötig, 5–10 FPS reichen für Katzen-Erkennung am Napf

- [ ] Radxa CM3 Datenblatt + CM4-Carrier-Board Referenzdesign herunterladen (radxa.com/cm3)
- [ ] Schaltplan in KiCad erstellen (Carrier Board, CM4-kompatibler Formfaktor)
- [ ] PCB-Layout erstellen (4-lagig, Impedanzkontrolle für MIPI CSI + USB)
- [ ] Gerber-Dateien an JLCPCB / PCBWay senden
- [ ] Bauteile bestellen:
  - Radxa CM3 (RK3566, 1GB RAM, WiFi/BT) — **~35 €** (radxa.com / AliExpress)
  - CM4-kompatibler Stecker (2× 100-pin Hirose DF40) — **~4 €**
  - ~~Arducam IMX219 NoIR Kamera — **~25 €**~~ → **vorhanden** (RPi NoIR Camera V2, kompatibler CSI-2 Stecker)
  - FPC-Stecker 15-pin, 1.0mm Raster — **~1 €**
  - HX711 SOIC-16 (Waagen-ADC) — **~1–2 €**
  - Wägezelle 1kg TAL220B — **~10 €**
  - TPS54331DR SOIC-8 Buck Converter (×2) — **~4 €**
  - USB-C Buchse — **~2 €**
  - CP2102N-A02-GQFN24 USB-UART Bridge — **~4 €**
  - BAT54S SOT-23 Schottky-Diode — **~0,50 €**
  - Kondensatoren, Widerstände, LEDs, Taster (Sortiment) — **~8 €**
  - PCB-Fertigung 4-lagig (JLCPCB, 5 Stück) — **~20 €**
  - **Gesamt: ~90 €** (Kamera bereits vorhanden)
- [ ] PCB bestücken und löten

## Software-Setup (Radxa CM3)

- [ ] Debian / Ubuntu auf Radxa CM3 aufsetzen
- [ ] Python + PyTorch + Ultralytics installieren
- [ ] NoIR-Kamera (RPi NoIR Camera V2) per MIPI CSI einrichten
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
