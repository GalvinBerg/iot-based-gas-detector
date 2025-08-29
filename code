# iot-based-gas-detector
#include <ESP8266WiFi.h>
#include <PubSubClient.h>

// WiFi and MQTT setup
const char* ssid = "YOUR_WIFI_SSID";
const char* password = "YOUR_WIFI_PASSWORD";
const char* mqtt_server = "MQTT_BROKER_ADDRESS";
const int mqtt_port = 1883;
const char* mqtt_user = "MQTT_USERNAME";
const char* mqtt_password = "MQTT_PASSWORD";
const char* mqtt_topic = "gas_leakage/data";

WiFiClient espClient;
PubSubClient client(espClient);

// Gas sensor pin
const int gasSensorPin = A0;

// Variables for filtering
const int numReadings = 10;
int readings[numReadings];
int readIndex = 0;
int total = 0;
int average = 0;

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Attempting MQTT connection...");
    if (client.connect("ESP8266Client", mqtt_user, mqtt_password)) {
      Serial.println("connected");
    } else {
      Serial.print("failed, rc=");
      Serial.print(client.state());
      Serial.println(" try again in 5 seconds");
      delay(5000);
    }
  }
}

void setup() {
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqtt_server, mqtt_port);

  // Initialize readings array
  for (int i = 0; i < numReadings; i++) {
    readings[i] = 0;
  }
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();

  // Read gas sensor value
  total = total - readings[readIndex];
  readings[readIndex] = analogRead(gasSensorPin);
  total = total + readings[readIndex];
  readIndex = (readIndex + 1) % numReadings;
  average = total / numReadings;

  Serial.print("Average Gas Value: ");
  Serial.println(average);

  // Publish average gas value to MQTT topic
  char gasValueStr[10];
  sprintf(gasValueStr, "%d", average);
  client.publish(mqtt_topic, gasValueStr);

  delay(1000); // Send data every 1 second
}
