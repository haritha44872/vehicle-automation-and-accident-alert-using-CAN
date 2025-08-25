#include <SoftwareSerial.h>

SoftwareSerial gsm(9, 10); // RX, TX

int vibrationSensor = 2;
int fireSensor = 3;
int waterSensor = 4;
int buzzer = 5;

void setup() {
  pinMode(vibrationSensor, INPUT);
  pinMode(fireSensor, INPUT);
  pinMode(waterSensor, INPUT);
  pinMode(buzzer, OUTPUT);
  
  Serial.begin(9600);
  gsm.begin(9600);
  
  delay(1000);
  sendSMS("System Initialized and Monitoring Started.");
}

void loop() {
  int vibration = digitalRead(vibrationSensor);
  int fire = digitalRead(fireSensor);
  int water = digitalRead(waterSensor);

  if (vibration == HIGH) {
    alert("Vibration Detected! Possible Accident.");
  }

  if (fire == HIGH) {
    alert("Fire Detected! Emergency Alert.");
  }

  if (water == HIGH) {
    alert("Water Level Critical! Possible Flooding.");
  }

  delay(1000);
}

void alert(const char* message) {
  digitalWrite(buzzer, HIGH);
  sendSMS(message);
  delay(5000);
  digitalWrite(buzzer, LOW);
}

void sendSMS(const char* message) {
  gsm.println("AT+CMGF=1");
  delay(1000);
  gsm.println("AT+CMGS=\"+919999999999\""); // Replace with the recipient's phone number
  delay(1000);
  gsm.print(message);
  delay(1000);
  gsm.write(26); // Ctrl+Z to send SMS
  delay(3000);
}
