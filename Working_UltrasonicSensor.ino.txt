const int trigPin = 9;
const int echoPin = 8;
const int LED_PIN  = 3;

void setup() {
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
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
  } else {
      Serial.print("Distance: ");
      Serial.print(distance_cm);
      Serial.println(" cm");
  }

  delay(500);
}




/*const int trigPin = 9;
const int echoPin = 8;

void setup() {
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
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

  // Print the distance to the LCD screen
  char lcd_buffer[16]; // Buffer for storing the LCD text
  sprintf(lcd_buffer, "Distance: %.1f cm", distance_cm);
  Serial.println(lcd_buffer);
  delay(500);
}*/
