#include <Servo.h>

// Pin definitions
#define TRIG_PIN 9
#define ECHO_PIN 10
#define SERVO_PIN 6

// Create servo object
Servo lidServo;

void setup() {
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  lidServo.attach(SERVO_PIN);
  lidServo.write(0);  // Close lid initially
  Serial.begin(9600);
}

void loop() {
  long duration;
  int distance;

  // Trigger the ultrasonic sensor
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  // Read the echo pin
  duration = pulseIn(ECHO_PIN, HIGH);
  distance = duration * 0.034 / 2;  // Convert to cm

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // If object detected within 20 cm, open the lid
  if (distance > 0 && distance <= 20) {
    lidServo.write(90);  // Open lid
    delay(5000);         // Keep open for 5 seconds
    lidServo.write(0);   // Close lid
  }

  delay(500);  // Delay before next measurement
}
