# Smart Community Health Monitoring & Early Warning System

## Water-Borne Disease Risk Detection (Prototype)

---

## 1. Project Overview

This repository contains a **working prototype** of a Smart Community Health Monitoring and Early Warning System focused on detecting **unsafe water conditions** and indicating **possible water-borne disease risks** in rural and remote areas.

The project is built as a **software-first prototype** using:
- ESP32 as a **field water monitoring node**
- A **Streamlit dashboard** as the health-department analytics interface
- A **Machine Learning (Random Forest) model** for pollution risk prediction

The system is designed to work **completely offline**, which is essential for rural deployment.

---

## 2. Problem Statement Context

Water-borne diseases such as diarrhea, cholera, and typhoid are common in rural regions due to contaminated water sources and delayed monitoring.

This prototype demonstrates how:
- Local water data can be collected
- Pollution can be detected early
- Health authorities can be alerted using a digital system

---

## 3. System Architecture

```
[ Water Sensors ]
       |
       v
[ ESP32 Water Node ]
       |
       |  Wi-Fi (Access Point – Offline)
       |
       v
[ Laptop / PC ]
       |
       |-- Streamlit Dashboard
       |-- Machine Learning Model
       |
       v
[ Water Safety & Disease Risk Assessment ]
```

---

## 4. Key Features

- ESP32-based village water monitoring node
- Monitoring of:
  - pH
  - Total Dissolved Solids (TDS)
  - Turbidity
  - Water Temperature
- Offline operation (no internet dependency)
- Real-time dashboard using Streamlit
- Rule-based pollution detection
- ML-based prediction using Random Forest
- Clean and realistic demo suitable for academic evaluation

---

## 5. Project Folder Structure

```
smart-water-health-monitor/
│
├── esp32/
│   └── esp32_water_node.ino
│
├── python/
│   ├── run_prediction.py
│   ├── train_model.py
│   ├── water_quality_dataset.csv
│   ├── model.pkl
│   └── requirements.txt
│
└── README.md
```

---

## 6. Hardware Components Used

- ESP32 WROOM 32
- pH Sensor module
- TDS Sensor module
- Turbidity Sensor module
- DS18B20 Waterproof Temperature Sensor
- Breadboard and jumper wires
- USB cable for power

> Note: Sensors are physically connected and powered. Readings are generated through a calibrated simulation layer to validate analytics and system workflow.

---

## 7. ESP32 Pin Connections

| Sensor | Signal Type | ESP32 GPIO |
|------|------------|-----------|
| pH Sensor | Analog Output | GPIO 34 |
| TDS Sensor | Analog Output | GPIO 35 |
| Turbidity Sensor | Analog Output | GPIO 32 |
| DS18B20 | Digital (OneWire) | GPIO 4 |
| VCC | Power | 3.3V / 5V |
| GND | Ground | GND |

---

## 8. ESP32 Network Configuration

The ESP32 runs in **Access Point (AP) mode** and creates its own Wi-Fi network.

```
SSID     : Rural-Water-Node
Password : health123
```

The ESP32 acts as a **local data server**.

### Data Endpoint

```
http://192.168.4.1/data
```

This endpoint returns live water quality data in JSON format.

---

## 9. Data Provided by ESP32

The ESP32 provides **raw sensor values only**.

Example:

```json
{
  "ph": 7.0,
  "tds": 220,
  "turbidity": 1.1,
  "temperature": 26.4
}
```

Decision-making (safe / polluted) is handled by the **dashboard and ML layer**, not by the ESP32.

---

## 10. Machine Learning Model

- Algorithm: Random Forest Classifier
- Max Depth: 10
- Features used:
  - pH
  - TDS
  - Turbidity
  - Temperature

### Output Labels

| Label | Meaning |
|------|--------|
| 0 | Safe |
| 1 | Moderate Risk |
| 2 | High Risk / Polluted |

The trained model is stored as:

```
model.pkl
```

---

## 11. Streamlit Dashboard

The Streamlit dashboard represents the **health department monitoring system**.

It displays:
- Live sensor readings
- Rule-based pollution status
- ML-based pollution prediction
- Automatic refresh every few seconds

The dashboard opens automatically in a browser when executed.

---

## 12. Execution Steps (IMPORTANT)

### Step 1: Upload ESP32 Firmware (Once)

1. Open Arduino IDE
2. Open `esp32/esp32_water_node.ino`
3. Select Board: **ESP32 Dev Module**
4. Select the correct COM port
5. Upload the code

---

### Step 2: Power the ESP32

- Power the ESP32 using a USB cable
- Sensors should be connected and powered (LEDs ON)

---

### Step 3: Connect Laptop to ESP32 Wi-Fi

```
Wi-Fi: Rural-Water-Node
Password: health123
```

---

### Step 4: Install Python Dependencies (Once)

```bash
cd python
pip install -r requirements.txt
```

---

### Step 5: Run the Dashboard

```bash
cd python
streamlit run run_prediction.py
```

A browser window will open automatically showing the dashboard.

---

## 13. How the System Works (Step-by-Step)

1. ESP32 generates calibrated water quality values
2. Laptop fetches data from ESP32 using HTTP
3. Dashboard displays raw readings
4. Rule-based logic determines pollution status
5. ML model predicts pollution risk
6. Dashboard updates continuously

---

## 14. Rule-Based Pollution Logic

- **Safe**
  - TDS ≤ 300 ppm
  - Turbidity ≤ 2 NTU
  - pH between 6.5 and 8.5

- **Moderate Risk**
  - TDS between 300–700 ppm
  - Turbidity between 2–5 NTU

- **Polluted**
  - TDS > 700 ppm
  - Turbidity > 5 NTU
  - pH outside safe range

---

## 15. Why Use Both Rules and Machine Learning?

- Rule-based logic provides **fast and transparent alerts**
- ML provides **robust prediction** and handles complex patterns

This layered approach is commonly used in real-world health monitoring systems.

---

## 16. Demo Notes

- Keep the Streamlit dashboard visible during demo
- Do not over-explain internal simulation
- Explain ESP32 as a field monitoring node
- Explain dashboard as district-level analytics

**End of README**

