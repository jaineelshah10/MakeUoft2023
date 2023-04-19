const int trigPin = 7;
const int echoPin = 6;
const int LED_PIN  = 3;

int red = 10;
int yellow = 9;
int green = 8;

int delayGreen = 10000;
int delayYellow = 3000;
int delayRed = 10000;

enum TrafficState {
  GREEN,
  GREEN_YELLOW,
  YELLOW,
  RED
};

TrafficState state = GREEN;

unsigned long previousMillis = 0;
const unsigned long intervalGreen = 10000;
const unsigned long intervalGreenYellow = 2000;
const unsigned long intervalYellow = 3000;
const unsigned long intervalRed = 10000;

void setup() {
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(green, OUTPUT);
  pinMode(yellow, OUTPUT);
  pinMode(red, OUTPUT);
}

void ultra() {
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  long duration_us = pulseIn(echoPin, HIGH);
  float distance_cm = duration_us / 58.0;

  Serial.print("Distance: ");
  Serial.print(distance_cm);
  Serial.println(" cm");

  if (distance_cm <= 20) {
    Serial.println("Vehicle incoming, turn not safe");
    digitalWrite(LED_PIN, LOW);
    delay(500);
  }
}

void greenLight() {
  digitalWrite(green, HIGH);
  digitalWrite(yellow, LOW);
  digitalWrite(red, LOW);
  digitalWrite(LED_PIN, HIGH);
}

void greenYellowLight() {
  digitalWrite(green, HIGH);
  digitalWrite(yellow, HIGH);
  digitalWrite(red, LOW);
}

void yellowLight() {
  digitalWrite(green, LOW);
  digitalWrite(yellow, HIGH);
  digitalWrite(red, LOW);
}

void redLight() {
  digitalWrite(green, LOW);
  digitalWrite(yellow, LOW);
  digitalWrite(red, HIGH);
}

void loop() {
  unsigned long currentMillis = millis();
  
  switch(state) {
    case GREEN:
      ultra();
      greenLight();
      if (currentMillis - previousMillis >= intervalGreen) {
        state = GREEN_YELLOW;
        previousMillis = currentMillis;
      }
      break;
      
    case GREEN_YELLOW:
      greenYellowLight();
      digitalWrite(LED_PIN, LOW);
      if (currentMillis - previousMillis >= intervalGreenYellow) {
        state = YELLOW;
        previousMillis = currentMillis;
      }
      break;
      
    case YELLOW:
      yellowLight();
      digitalWrite(LED_PIN, LOW);
      if (currentMillis - previousMillis >= intervalYellow) {
        state = RED;
        previousMillis = currentMillis;
      }
      break;
      
    case RED:
      redLight();
      digitalWrite(LED_PIN, LOW);
      if (currentMillis - previousMillis >= intervalRed) {
        state = GREEN;
        previousMillis = currentMillis;
      }
      break;
  }
}





