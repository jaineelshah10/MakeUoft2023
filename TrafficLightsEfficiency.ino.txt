#include <Wire.h>
#include <SoftwareSerial.h>
#include "rgb_lcd.h"

rgb_lcd lcd;
SoftwareSerial lcdSerial(4, 5);

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
  GREENLED,
  GREEN_YELLOWLED,
  YELLOWLED,
  REDLED
};

TrafficState state = GREENLED;

unsigned long previousMillis = 0;
const unsigned long intervalGreen = 10000;
const unsigned long intervalGreenYellow = 2000;
const unsigned long intervalYellow = 3000;
const unsigned long intervalRed = 10000;

void setup() {
  Serial.begin(9600);

  lcdSerial.begin(9600);
  lcd.begin(16, 2, &lcdSerial);
  lcd.setCursor(0,0);
  lcd.println("Traffic Light Efficiency - Group 616A4");

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(green, OUTPUT);
  pinMode(yellow, OUTPUT);
  pinMode(red, OUTPUT);
  pinMode(LED_PIN, OUTPUT); // initialize LED_PIN as an output
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
    delay(3000);
  }
}

void greenLight() {
  digitalWrite(green, HIGH);
  digitalWrite(yellow, LOW);
  digitalWrite(red, LOW);
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
  digitalWrite(LED_PIN, HIGH); // turn on LED_PIN only when red light is on
}

void loop() {

  lcd.scrollDisplayLeft();
  delay(400);
  unsigned long currentMillis = millis();
  
  switch(state) {
    case GREENLED:
      greenLight();
      digitalWrite(LED_PIN, LOW);
      if (currentMillis - previousMillis >= intervalGreen) {
        state = GREEN_YELLOWLED;
        previousMillis = currentMillis;
      }
      break;
      
    case GREEN_YELLOWLED:
      greenYellowLight();
      digitalWrite(LED_PIN, LOW);
      if (currentMillis - previousMillis >= intervalGreenYellow) {
        state = YELLOWLED;
        previousMillis = currentMillis;
      }
      break;
      
    case YELLOWLED:
      yellowLight();
      digitalWrite(LED_PIN, LOW);
      if (currentMillis - previousMillis >= intervalYellow) {
        state = REDLED;
        previousMillis = currentMillis;
      }
      break;
      
    case REDLED:
      ultra();
      redLight();
      if (currentMillis - previousMillis >= intervalRed) {
        state = GREENLED;
        previousMillis = currentMillis;
      }
      break;
  }
}


