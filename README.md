# 🌊 BlauEdge — Marine Ecosystem Recovery Monitor

> **HackUPC 2026** · EdgeAI for a Resilient and Greener Barcelona  
> Built with Arduino UNO Q · Edge Impulse · Python · HTML

---

## 💡 Inspiration

Recent efforts in Barcelona showed that over **100 local marine species returned to the coastline in just a few months**. That rapid recovery made us realise how resilient marine ecosystems can be — and also how little real-time data we have to monitor and guide that recovery.

Barcelona's breakwaters (*escolleres*) could be thriving underwater sanctuaries, but right now nobody is watching them. No one knows which sections are recovering and which are biological deserts. We built BlauEdge to change that.

---

## 🌍 What It Does

BlauEdge is a real-time marine ecosystem monitoring system that evaluates the health of coastal zones along Barcelona's coastline using two inputs:

- **Temperature sensing** — detects thermal stress events that signal ecosystem decline
- **AI species detection** — uses a camera and an on-device Edge Impulse model to identify marine species on the breakwater surface

The results are streamed live to an **interactive web dashboard** with a real map of the Barcelona coastline, showing the health state of each monitored zone in real time.

A **simulation mode** keeps the dashboard fully functional even without hardware connected.

---

## 🔧 How We Built It


1. The **Arduino UNO Q** reads the temperature sensor and runs the Edge Impulse species detection model on camera frames
2. It outputs one CSV line per reading over USB serial
3. A **Python script** reads that serial stream, classifies the ecosystem health state, and serves it as a local JSON API
4. The **HTML dashboard** polls that API every second and updates the live map and sensor readings in real time

---

## 🔌 Hardware

| Component | Role |
|---|---|
| **Arduino UNO Q** | Main brain — runs Edge Impulse model on-device |
| **Modulino Thermo** | Water temperature — detects thermal stress events |
| **USB Web Camera** | Captures breakwater surface for species detection |

---

## 🤖 AI Model

Built with **Edge Impulse Studio**, deployed on the Arduino UNO Q.

- **Type:** FOMO object detection (Faster Objects, More Objects)
- **Input:** Camera frames of the breakwater surface
- **Output:** Detected marine species present in the frame
- **Training data:** Manually collected and labelled images — no public dataset of local Barcelona coastal species exists, so we built our own from scratch

The detected species count, combined with the temperature reading, feeds a simple classifier that assigns one of four health states:

| State | Meaning |
|---|---|
| 🟢 **HEALTHY** | Good temperature, species detected |
| 🔵 **RECOVERING** | Moderate conditions, some biological activity |
| 🔴 **STRESSED** | Temperature spike above threshold |
| ⚫ **INACTIVE** | No species detected, unfavourable conditions |

---

## 🏗️ Software Architecture

```
blauedge/
├── sketch.ino     # Arduino — reads temp + runs camera model, outputs CSV
├── bridge.py      # Python — serial reader + health classifier + REST API
├── index.html     # Dashboard — live map, sensor data, field notes
└── README.md
```


### API (Python → Browser)

| Endpoint | Method | What it returns |
|---|---|---|
| `/api/data` | GET | All node readings + notes |
| `/api/nodes` | POST | Add / update a node |
| `/api/notes` | POST | Add a field note |
| `/api/notes/<id>` | DELETE | Remove a note |

---

## 😓 Challenges

**Camera + Arduino OS clash** — connecting the Logitech USB camera to the Arduino caused driver conflicts due to OS incompatibilities. Took significant debugging to resolve.

**Arduino → browser in real time** — getting live hardware data into a browser was much harder than expected. The Python bridge layer was our solution, but building and debugging the full pipeline under time pressure was the hardest part of the project.

**No local species dataset** — Edge Impulse needs labelled training data. Since no dataset of Barcelona coastal marine species exists, we had to collect and label images manually.

---

## 🚀 Getting Started

### Requirements

- Python 3.10+
- Arduino UNO Q + Modulino Thermo + USB camera
- [Arduino App Lab IDE](https://docs.arduino.cc/software/app-lab/)
- [Edge Impulse Studio](https://studio.edgeimpulse.com/)

### Install

```bash
git clone https://github.com/YOUR_TEAM/blauedge-hackupc2026
cd blauedge-hackupc2026
pip install flask flask-cors pyserial
```

### Run with Arduino

```bash
# Windows
python bridge.py --port COM3

# Mac / Linux
python bridge.py --port /dev/ttyACM0
```

### Run in demo mode (no hardware needed)

```bash
python bridge.py --demo
```

Open `index.html` in your browser — it connects to `http://localhost:5000` automatically.

### Flash the Arduino

1. Open `sketch.ino` in Arduino App Lab IDE
2. Select **Arduino UNO Q** as target
3. Set your `NODE_ID` (e.g. `"A1"`)
4. Upload

---

## 🔮 What's Next

- **Real species dataset** — collect labelled images of actual native Barcelona coastal species for a much more accurate model
- **More sensors** — pH, light intensity, vibration/acoustic, salinity to build a full ecosystem fingerprint
- **Multi-node deployment** — cover the full Barceloneta coastline
- **Waterproof enclosure** — for real underwater breakwater deployment
- **Scale** — make BlauEdge replicable for any coastal city

---

## 👥 Team

| Name | Role |
|---|---|
| [Agustina Ciaponi] | Hardware + Arduino |
| [Emma Leroux Fernandez] | Edge Impulse model |
| [Marti Amat] | Python bridge |
| [Guillem Arevalo Morell] | Dashboard + frontend |

**HackUPC 2026**

---

## 📜 License

see [LICENSE.txt](LICENSE.txt)

---

## 🔗 Links

- 🌐 [Devpost](https://hackupc-2026.devpost.com/) ← add link
- 🛠️ [Arduino Project Hub](https://projecthub.arduino.cc/) ← add link
- 🤖 [Edge Impulse Project](https://studio.edgeimpulse.com/) ← add link
- 📹 Demo Video ← add link

---

*Built with ❤️ for Barcelona's coast at HackUPC 2026*
*"The ocean is not a dump. It's a garden." — Let's tend it.*
