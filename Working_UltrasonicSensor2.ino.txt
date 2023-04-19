const int trigPin = 9;
const int echoPin = 8;
const int LED_PIN = 3;

void setup() {
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(LED_PIN, OUTPUT); // Set LED pin as output
}

void loop() {
  // Generate 10-microsecond pulse to TRIG pin
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Measure duration of pulse from ECHO pin
  long duration_us = pulseIn(echoPin, HIGH);

  // Calculate the distance
  float distance_cm = duration_us / 58.0;

  // Print the distance to the serial monitor
  Serial.print("Distance: ");
  Serial.print(distance_cm);
  Serial.println(" cm");

  // Check if the distance is less than or equal to 20 cm
  if (distance_cm <= 20) {
    Serial.println("Vehicle incoming, turn not safe");
    digitalWrite(LED_PIN, LOW); // Turn off LED
  } else {
    Serial.print("Distance: ");
    Serial.print(distance_cm);
    Serial.println(" cm");
    digitalWrite(LED_PIN, HIGH); // Turn on LED
  }

  delay(500);
}
