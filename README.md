#include <Arduino.h>

#define BLYNK_AUTH_TOKEN "7ZJpao40OD9-0SLuCUrDFU-k60wPYaF4"
#define BLYNK_TEMPLATE_NAME "cloud weather station "
#define BLYNK_TEMPLATE_ID "TMPL3P9uhu61K"
#include <BlynkSimpleEsp32.h>
#include <DHT.h>
#include <WiFi.h>
const char* ssid = "realme GT 6";
const char* pass="q5y6j9np";

#define dhttype DHT11
#define dhtpin 4

DHT dht(dhtpin, dhttype);

BlynkTimer timer;

void sendServer(){
  float t = dht.readTemperature();
  float h = dht.readHumidity();

  if(isnan(t)||isnan(h)){
    Serial.println("Failed to read sensor data.");
    return;
  }

  Blynk.virtualWrite(V0, t);
  Blynk.virtualWrite(V1, h);

  Serial.print("Teamperature: ");
  Serial.println(t);
}

void setup(){
  Serial.begin(115200);
  dht.begin();
  Blynk.begin(BLYNK_AUTH_TOKEN,ssid,pass);
  timer.setInterval(2000L,sendServer);
}

void loop(){
  Blynk.run();
  timer.run();
}
