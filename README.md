#define BLYNK TEMPLATE ID "TMPL3D-56E25T"
#define BLYNK TEMPLATE NAME "Petrol Adulteration detection System"
#define BLYNK AUTH TOKEN "IxE30T4j3KjIfB-XYdJUBmMw_SEWyzf6"
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
#include "HX711.h"
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
char ssid[]= "Kaypn.com";
char pass[]= "12345678";
#define DT 4
#define SCK 5
#define RELAY 18
HX711 scale:
LiquidCrystal I2C lcd(0x27, 16, 2);
float calibration factor = -7050;
void setup()
Serial.begin(9600);
Blynk.begin(BLYNK AUTH_TOKEN, ssid, pass);
lcd.init();
lcd.backlight();
scale.begin(DT, SCK);
scale.set_scale(calibration_factor);
scale.tare();
pinMode(RELAY, OUTPUT);
lcd.setCursor(0,0);
Icd.print("Fuel Analyzer");
lcd.setCursor(0,1);
led.print("Initializing...");
delay(2000);
lcd.clear();
void loop()
{
Blynk.run();
float mass =scale.get_units(10);
if(mass <0) mass = 0;
String fuelType ="Unknown";
lcd.setCursor(0,0);
lcd.print("Wt:");
lcd.print(mass,2);
led.print(" g ");
if(mass <5)
{
}
fuelType = "Put the Fuel"
digitalWrite(RELAY, LOW);
// KEROSENE MIX (19.35 - 19.40 g)
else if(mass >= 18.00 && mass <= 19.50)
fuelType = "Kerosene Mix";
digitalWrite(RELAY, HIGH);
// PURE PETROL (20.26 - 20.31 g)
else if(mass >= 19.60 && mass <= 20.80)
fuelType = "Pure Petrol";
digitalWrite(RELAY, LOW);
// ETHANOL MIX (20.95- 21.04 g)
else if(mass >= 20.90 && mass <= 24.04) {
fuelType = "Ethanol Mix";
digitalWrite(RELAY, HIGH);
}
else
{
fuelType = "Out of Range";
digitalWrite(RELAY, LOW);
}
//===== LCD Second Line =====
lcd.setCursor(0,1);
lcd.print(fuelType);
lcd.print(" ");
//== Blynk Data
Blynk.virtualWrite(V0, mass);
// Gauge / Value widget
Blynk.virtualWrite(V1, fuelType); // Label widget
//==== Serial Monitor
Serial.print("Mass: ");
Serial.print(mass);
Serial.print(" g | Type: ");
Serial.println(fuelType);
delay(1000); // faster update
}
