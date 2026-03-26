# CatFeederAI

Automatisches Tracking der Futtermengen von Hauskatzen mithilfe von Computer Vision und einer Waage. Das System erkennt in Echtzeit welche Katze frisst und speichert die gefressene Menge in einer Datenbank.

---

## Funktionsweise

Eine NoIR-Kamera beobachtet den Futternapf kontinuierlich. Sobald eine Katze erkannt wird, wird das aktuelle Gewicht der Waage gespeichert. Wenn die Katze den Napf verlässt, wird die Differenz berechnet und als Fütterungseintrag in der Datenbank abgelegt.

---

## Tech Stack

| Bereich | Technologie |
|---|---|
| Sprache | Python 3 |
| ML Framework | PyTorch, torchvision |
| Object Detection | YOLOv8 (Ultralytics) |
| Klassifikation | MobileNetV2 (Transfer Learning) |
| Computer Vision | OpenCV |
| Datenbank | PostgreSQL (psycopg2) |
| Hardware | Raspberry Pi, NoIR-Kamera, Waage |
| Notebooks | Jupyter |

---

## Angewandte Data Science Methoden

**Datenvorbereitung**
- Eigener Bilddatensatz mit zwei Klassen (je Katze)
- Stratified Train/Validation/Test Split (80/10/10)
- Data Augmentation: Random Flip, Rotation, Color Jitter, Random Crop

**Modellentwicklung**
- Transfer Learning auf MobileNetV2 (pretrained ImageNet)
- Fine-Tuning des Klassifikations-Layers für binäre Klassifikation
- Modellvergleich: MobileNetV2 vs. YOLOv11 vs. EfficientNet-Lite0 (nach Trainingszeit, Modellgröße, Genauigkeit)
- Early Stopping (patience=5) zur Überanpassungsvermeidung

**Ergebnisse MobileNetV2**
- Validation Accuracy: **94.85%**
- Validation Loss: 0.0975
- Modellgröße: 8.8 MB
- Trainingszeit: ~17 Minuten

**Inferenz-Pipeline**
- Zweistufige Echtzeit-Erkennung: YOLO (Detektion) → MobileNetV2 (Klassifikation)
- Bounding Box Cropping vor Klassifikation
- GPU-Unterstützung (CUDA, fallback CPU)

---

## Projektstruktur

```
CatFeederAI/
├── train_test_val_split.ipynb   # Datensatz aufteilen
├── train_model.ipynb            # Modelltraining & Evaluation
├── webcam.ipynb                 # Live-Inferenz Pipeline
├── commit_to_database.ipynb     # Datenbankverbindung & Schema
└── main.ipynb                   # Hauptprogramm (in Entwicklung)
```
