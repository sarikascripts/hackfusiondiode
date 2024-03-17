# hackfusiondiode
#define LED1_PIN 8 // Pin for LED1
#define LED2_PIN 11 // Pin for LED2
#define LED3_PIN 10  // Pin for LED3
int enB = 3;
int in3 = 5;
int in4 = 4;

#include "DHT.h"
#define DHTPIN 2     // Digital pin connected to the DHT sensor

#define DHTTYPE DHT11   // DHT 11
DHT dht(DHTPIN, DHTTYPE);

void setup() {
  pinMode(LED1_PIN, INPUT); // Set LED1 pin as an output
  pinMode(LED2_PIN, OUTPUT); // Set LED2 pin as an output
  pinMode(LED3_PIN, OUTPUT); // Set LED3 pin as an output
  pinMode(enB, OUTPUT);
	pinMode(in3, OUTPUT);
	pinMode(in4, OUTPUT);
	digitalWrite(in3, LOW);
	digitalWrite(in4, LOW);
  delay(1500);
   Serial.begin(9600);
  Serial.println(F("DHTxx test!"));

  dht.begin();

  
}

void loop() {
  
  digitalWrite(LED3_PIN, HIGH); // Turn on LED3

  // Check if LED1 is ON
  if (digitalRead(LED1_PIN) == LOW) {
    digitalWrite(LED2_PIN, LOW); 
    analogWrite(enB, 255);
   digitalWrite(in3, HIGH);
  	digitalWrite(in4, LOW);

  } else {
    digitalWrite(LED2_PIN, HIGH); 
    delay(100);
    digitalWrite(LED2_PIN, LOW); 
    delay(100);
    analogWrite(enB, 255);
   digitalWrite(in3, LOW);
  	digitalWrite(in4, LOW);
  }
 
	
  float h = dht.readHumidity();
  float t = dht.readTemperature();
  float f = dht.readTemperature(true);
  if (isnan(h) || isnan(t) || isnan(f)) {
    Serial.println(F("Failed to read from DHT sensor!"));
    return;
  }
  float hif = dht.computeHeatIndex(f, h);
  float hic = dht.computeHeatIndex(t, h, false);
  Serial.print(F("Humidity: "));
  Serial.print(h);
  Serial.print(F("%  Temperature: "));
  Serial.print(t);
  Serial.print(F("°C "));
  Serial.print(f);
  Serial.print(F("°F  Heat index: "));
  Serial.print(hic);
  Serial.print(F("°C "));
  Serial.print(hif);
  Serial.println(F("°F"));

}
