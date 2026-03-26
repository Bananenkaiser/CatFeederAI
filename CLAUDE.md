# CatFeederAI — Projekt-Kontext für Claude

## Projektidee

Automatisches Tracking der Futtermengen von zwei Katzen (**Elsa** und **Fabius**).

**Ablauf:**
1. Eine NoIR-Kamera erkennt, wenn sich eine Katze dem Futternapf nähert
2. YOLOv8 detektiert die Katze im Live-Stream (COCO class 15 = cat)
3. MobileNetV2 klassifiziert, welche Katze es ist (Elsa oder Fabius)
4. Beim Erscheinen der Katze → aktuelles Gewicht der Waage speichern
5. Beim Verschwinden der Katze → neues Gewicht messen → Differenz = gefressene Menge
6. Eintrag in PostgreSQL-Datenbank schreiben

---

## Architektur

```
NoIR Kamera
    │
    ▼
YOLOv8n (COCO-Modell, yolov8n.pt)
  → Erkennt ob eine Katze im Bild ist
  → Liefert Bounding Box
    │
    ▼
MobileNetV2 (catid_mobilenetv2.pt)
  → Klassifiziert: Elsa (0) oder Fabius (1)
  → Genauigkeit: ~94.85% auf Validierungsset
    │
    ▼
Waage
  → Gewicht beim Erscheinen der Katze merken
  → Differenz beim Verschwinden berechnen
    │
    ▼
PostgreSQL (auf NAS / Cloud_9)
  → Tabelle: cat_feeding
  → Felder: id, timestamp, cat_id, weight
```

---

## Dateien

| Datei | Zweck |
|---|---|
| `main.ipynb` | Platzhalter / Einstiegspunkt (noch leer) |
| `train_test_val_split.ipynb` | Dataset 80/10/10 aufteilen (Elsa & Fabius) |
| `commit_to_database.ipynb` | PostgreSQL-Verbindung testen & Tabelle anlegen |
| `train_model.ipynb` | MobileNetV2 Training (fertig, Ergebnisse gespeichert) |
| `webcam.ipynb` | Live-Inferenz: YOLO + MobileNetV2 auf Webcam-Feed |
| `catid_mobilenetv2.pt` | Trainiertes Modell (8.8 MB) — nicht im Git |
| `yolov8n.pt` | YOLOv8 Nano (6.3 MB) — nicht im Git |
| `db_config.json` | DB-Zugangsdaten — nicht im Git (.gitignore) |
| `idee.txt` | Originale Projektidee und offene Fragen |

**Lokale Daten (nicht im Git):**
- `cats/Elsa/` — 512 Bilder
- `cats/Fabius/` — 347 Bilder
- `data_split/train|val|test/` — 860 Bilder aufgeteilt

NAS-Pfad (intern): `\\Cloud_9\data\pet_feeder_ai\`

---

## Modell-Details

### MobileNetV2 (Katzen-Klassifikation)
- Pretrained auf ImageNet, letzter Layer auf 2 Klassen angepasst
- Input: 224×224, ImageNet-Normalisierung
- Training: Adam lr=1e-4, CrossEntropyLoss, Early Stopping patience=5
- Ergebnis: Val-Loss 0.0975, Val-Accuracy **94.85%**
- Training dauerte ~17 Minuten

### YOLOv8n (Katzen-Detektion)
- Standard COCO-Modell, kein Fine-Tuning
- Klasse 15 = cat

### Preprocessing (Inferenz)
```python
transforms.Compose([
    transforms.Resize((224, 224)),
    transforms.ToTensor(),
    transforms.Normalize(mean=[0.485, 0.456, 0.406],
                         std=[0.229, 0.224, 0.225]),
])
```

---

## Datenbank

```sql
CREATE TABLE cat_feeding (
    id        SERIAL PRIMARY KEY,
    timestamp TIMESTAMP NOT NULL,
    cat_id    INTEGER NOT NULL,   -- 0=Elsa, 1=Fabius
    weight    FLOAT NOT NULL      -- gefressene Menge in Gramm
);
```

Verbindungsparameter in `db_config.json` (aus Git ausgeschlossen).

---

## Offene Probleme (aus idee.txt)

1. **Zwei Fressnäpfe:** Wie beide gleichzeitig überwachen? → Evtl. ein großer Teller
2. **Gleichzeitiges Fressen:** Was wenn beide Katzen am selben Napf fressen?
3. **Futtersorte:** Automatische Erkennung der Futtersorte noch nicht implementiert
4. **Offline-Pufferung:** Wenn NAS nicht erreichbar → Einträge lokal puffern, bei nächster Verbindung senden

---

## Nächste Schritte

- [ ] Waagen-Integration (Hardware → Python)
- [ ] Hauptprogramm in `main.ipynb` zusammenbauen (YOLO + MobileNetV2 + Waage + DB)
- [ ] Offline-Pufferung implementieren (lokale SQLite → sync zu PostgreSQL)
- [ ] Mehrere Näpfe / gleichzeitiges Fressen behandeln
- [ ] Deployment auf Raspberry Pi (NoIR Kamera)

---

## Dependencies

```
torch, torchvision
ultralytics          # YOLOv8
opencv-python        # cv2
Pillow               # PIL
psycopg2-binary      # PostgreSQL
numpy
scikit-learn         # train_test_split
matplotlib
jupyter
```
