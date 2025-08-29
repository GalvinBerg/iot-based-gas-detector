🚨 IoT-Based Gas Detector using ESP8266 & MQTT

This project implements an IoT-based gas leakage detection system using an ESP8266 (NodeMCU), a gas sensor (e.g., MQ-2/MQ-135), and MQTT protocol for real-time monitoring.

The system continuously measures gas levels, filters noise using a moving average, and publishes the readings to an MQTT broker for integration with dashboards (like Node-RED, ThingsBoard, or Home Assistant).

🔧 Features

📡 Connects ESP8266 to Wi-Fi & MQTT broker.

🧪 Reads gas sensor values from the analog pin.

🔄 Applies a moving average filter for smooth readings.

📤 Publishes data to an MQTT topic (gas_leakage/data).

🔔 Can trigger alerts when thresholds are exceeded (extendable).

🛠️ Hardware Required

ESP8266 (NodeMCU / Wemos D1 Mini)

Gas sensor (MQ-2, MQ-135, etc.)

Breadboard & Jumper Wires

Power Supply (5V USB or Battery Pack)

📂 Project Structure
iot-gas-detector/
│── iot_gas_detector.ino   # Main Arduino code
│── README.md              # Project documentation

⚡ Circuit Diagram

Gas Sensor AO → ESP8266 A0

VCC → 5V

GND → GND

📡 MQTT Setup

Set up an MQTT broker (e.g., Mosquitto, HiveMQ, Eclipse Mosquitto on Raspberry Pi, or public MQTT broker).

Update these fields in the code with your credentials:

const char* ssid = "YOUR_WIFI_SSID";
const char* password = "YOUR_WIFI_PASSWORD";
const char* mqtt_server = "MQTT_BROKER_ADDRESS";
const int mqtt_port = 1883;
const char* mqtt_user = "MQTT_USERNAME";
const char* mqtt_password = "MQTT_PASSWORD";
const char* mqtt_topic = "gas_leakage/data";

📊 Sample Output

Serial Monitor output:

Average Gas Value: 320
Average Gas Value: 318
Average Gas Value: 322


MQTT Broker message (gas_leakage/data):

"320"
"318"
"322"

🚀 Usage

Flash the code to ESP8266 using Arduino IDE or PlatformIO.

Ensure MQTT broker is running and accessible.

Open Serial Monitor to check connection logs.

Subscribe to the MQTT topic (gas_leakage/data) using:

mosquitto_sub -h <broker_address> -t gas_leakage/data

📈 Future Improvements

Add threshold-based alert system (buzzer/LED).

Push data to cloud dashboards (ThingsBoard, Grafana, Blynk).

Send mobile notifications on gas leakage detection.

Integrate with home automation systems.
