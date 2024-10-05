#include <SoftwareSerial.h>
#include <TinyGPS++.h>

#define ALCOHOL_SENSOR_PIN A0
#define RELAY_PIN 4

// Use SoftwareSerial for GPS communication
SoftwareSerial gpsSerial(2, 3); // RX, TX
TinyGPSPlus gps;

void setup() {
  Serial.begin(9600);
  gpsSerial.begin(9600);
  pinMode(RELAY_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, LOW); // Start with the engine unlocked
}

void loop() {
  // Read GPS data
  while (gpsSerial.available() > 0) {
    gps.encode(gpsSerial.read());
  }

  // Check for GPS location update
  if (gps.location.isUpdated()) {
    Serial.print("Latitude: ");
    Serial.print(gps.location.lat(), 6);
    Serial.print(" Longitude: ");
    Serial.println(gps.location.lng(), 6);
  } else {
    Serial.println("Waiting for GPS signal...");
  }

  // Check alcohol level
  int sensorValue = analogRead(ALCOHOL_SENSOR_PIN);
  Serial.print("Alcohol Sensor Value: ");
  Serial.println(sensorValue);

  // Simple threshold for alcohol detection
  if (sensorValue > 200) { // Adjust threshold based on calibration
    Serial.println("Alcohol detected! Locking engine.");
    digitalWrite(RELAY_PIN, HIGH); // Lock the engine
  } else {
    Serial.println("No alcohol detected. Engine unlocked.");
    digitalWrite(RELAY_PIN, LOW); // Unlock the engine
  }

  delay(1000); // Delay for stability
}

<!---
dkdinesh3235/dkdinesh3235 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
