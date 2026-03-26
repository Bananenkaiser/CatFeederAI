# ToDo

## Hardware-Integration
- [ ] Waage per GPIO/I2C an Raspberry Pi anbinden
- [ ] Gewichtsauslese in Python implementieren
- [ ] Nullstellung der Waage per Knopf realisieren
- [ ] NoIR-Kamera am Raspberry Pi einrichten und testen

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
- [ ] System auf Raspberry Pi lauffähig machen
- [ ] Autostart beim Hochfahren konfigurieren
- [ ] Robustheit testen (Dauerbetrieb, Lichtbedingungen, Kamerawinkel)
