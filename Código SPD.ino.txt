#include <LiquidCrystal.h>

int motor1Pin = 11;
int motor2Pin = 10;
int ledRojo = 9;
int pinLDR = 0;
int luminosidad = 0;
float temperatura = 2;
int humedad = 1;
// RS,E,DB4,DB5,DB6,DB7
LiquidCrystal lcd(13, 8, 7, 4, 3, 2);
void setup() {
  pinMode(motor1Pin, OUTPUT);
  pinMode(motor2Pin, OUTPUT);
  pinMode(ledRojo, OUTPUT);
  pinMode(humedad, INPUT);
  pinMode(luminosidad, INPUT);
  pinMode(temperatura, INPUT);
  Serial.begin(9600);
  lcd.begin(16, 2);
  lcd.setCursor(0, 0);
  lcd.print("Sistema de riego");
  delay(2000);
  lcd.clear();
}
void loop() {
  luminosidad = analogRead(0);
  temperatura = map(analogRead(2), 0, 1023, -5000, 45000);
  temperatura = temperatura / 100;
  humedad = map(A1, 0, 876, 0, 100);
  humedad = analogRead(1);
  Serial.println(luminosidad);
  Serial.println(humedad);
  Serial.println(temperatura);
  if (temperatura > 30) {
    lcd.setCursor(0, 0);
    lcd.print("Temp:");
    lcd.setCursor(5, 0);
    lcd.print(temperatura);
    digitalWrite(motor1Pin, HIGH);
  } else {
    lcd.setCursor(0, 0);
    lcd.print("Temp:");
    lcd.setCursor(5, 0);
    lcd.print(temperatura);
    digitalWrite(motor1Pin, LOW);
  }
  if (humedad <= 80) {
    lcd.setCursor(0, 1);
    lcd.print("Hum: ");
    lcd.setCursor(5, 1);
    lcd.print(humedad);
    lcd.print("%");
    lcd.setCursor(9, 1);
    lcd.print("=> dry"); // seco = dry
    digitalWrite(motor2Pin, HIGH);
    delay(1000);
  } else {
    lcd.setCursor(9, 1);
    lcd.print("=> wet"); // wet = humedo
    digitalWrite(motor2Pin, LOW);
  }
  if (luminosidad < 700) {
    analogWrite(ledRojo, 50);
  } else {
    analogWrite(ledRojo, 255);
  }
}