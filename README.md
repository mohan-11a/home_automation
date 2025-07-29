# Home Automation – Remote Temperature Monitoring System

An Arduino-based home automation project designed to remotely monitor temperature using a Wi-Fi module and a digital temperature sensor.

---

## 🧾 Project Description

This project implements a **remote temperature monitoring system** as part of a basic home automation concept. It uses **Arduino Uno**, **ESP8266 Wi-Fi module**, and a **temperature sensor (e.g., DHT11 or LM35)** to collect and send real-time temperature data to a remote server or IoT platform.

The system allows users to monitor room or environment temperature from anywhere using the internet.

---

## 🔧 Components Used

- Arduino Uno  
- ESP8266 Wi-Fi Module  
- Temperature Sensor (DHT11 / LM35)  
- Jumper Wires  
- Power Supply (USB or Adapter)

---

## 💡 How It Works

1. The **temperature sensor** reads the surrounding temperature.
2. The **Arduino Uno** processes the data and sends it to the **ESP8266 Wi-Fi module**.
3. The **ESP8266** uploads the data to a cloud platform (e.g., ThingSpeak, Blynk, or Firebase).
4. The user can monitor the temperature in real-time through a mobile app or web dashboard.

---

## 📜 Arduino Code Example (DHT11 + ThingSpeak)

```cpp
#include <DHT.h>
#include <ESP8266WiFi.h>

#define DHTPIN 2
#define DHTTYPE DHT11

const char* ssid = "Your_SSID";
const char* password = "Your_PASSWORD";
const char* server = "api.thingspeak.com";
String apiKey = "Your_API_Key";

WiFiClient client;
DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(9600);
  WiFi.begin(ssid, password);
  dht.begin();

  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi...");
  }
  Serial.println("Connected!");
}

void loop() {
  float temp = dht.readTemperature();

  if (isnan(temp)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }

  if (client.connect(server, 80)) {
    String postStr = "api_key=" + apiKey + "&field1=" + String(temp);
    client.println("POST /update HTTP/1.1");
    client.println("Host: api.thingspeak.com");
    client.println("Connection: close");
    client.println("Content-Type: application/x-www-form-urlencoded");
    client.print("Content-Length: ");
    client.println(postStr.length());
    client.println();
    client.print(postStr);
    client.stop();
  }

  Serial.print("Temperature sent: ");
  Serial.println(temp);
  delay(20000);  // Delay between uploads
}

## 🎓 Academic Information

- 🔬 **Semester**: 4th Semester (Minor Project)
- 🏫 **College**: Aditya Engineering college
- 🎓 **Branch**: Electronics and Communication Engineering
