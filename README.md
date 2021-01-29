# Arduino weather station.
 PTT Digital Transformation Training Reg10. 
 25/Jan/2021-05/Feb/2021.

//---------------------------//
// Arduino weather station.
// By Jaran Keowma. 
// https://github.com/Jarank/Arduino-Training/
// Simple I2C test for ebay SH1106 128x64 oled.
#include <Wire.h>
#include "SSD1306Ascii.h"
#include "SSD1306AsciiWire.h"
#include "DHT.h"

DHT dht;

// 0X3C+SA0 - 0x3C or 0x3D
#define I2C_ADDRESS 0x3C

// Define proper RST_PIN if required.
#define RST_PIN -1

SSD1306AsciiWire oled;
//------------------------------------------------------------------------------
void setup() {
  Wire.begin();
  Wire.setClock(400000L);

Serial.begin(9600);
Serial.println();
Serial.println("Status\tHumidity (%)\tTemperature (C)\t(F)");

dht.setup(2); // data pin 2


#if RST_PIN >= 0
  oled.begin(&SH1106_128x64, I2C_ADDRESS, RST_PIN);
#else // RST_PIN >= 0
  oled.begin(&SH1106_128x64, I2C_ADDRESS);
#endif // RST_PIN >= 0

  oled.setFont(Adafruit5x7);

  uint32_t m = micros();
  oled.clear();
  oled.println("Hello PTT!");
  oled.println("Have a nice day. :)");
  oled.println();
  oled.set2X();
  oled.println("REG10");
  oled.set1X();
  oled.print("\nmicros: ");
  oled.print(micros() - m);
}
//------------------------------------------------------------------------------
void loop() {
  delay(dht.getMinimumSamplingPeriod());

float humidity = dht.getHumidity(); // ดึงค่าความชื้น
float temperature = dht.getTemperature(); // ดึงค่าอุณหภูมิ

Serial.print(dht.getStatusString());
Serial.print("\t");
Serial.print(humidity, 1);
Serial.print("\t\t");
Serial.print(temperature, 1);
Serial.print("\t\t");
Serial.println(dht.toFahrenheit(temperature), 1);

oled.setFont(Adafruit5x7);
uint32_t m = micros();
oled.clear();
//oled.println("Hello PTT!");  
//  oled.println();
  oled.set1X();
  oled.print("Humidity: ");
  oled.set2X();
  oled.print("");
  oled.println(humidity);
  oled.set1X();
  oled.println();
  oled.set1X();
  oled.println("Temperature:");
  oled.println();
  oled.set2X();
  oled.print("     ");
  oled.println(temperature);
  oled.set1X();  
  oled.print("\nby:Jaran Keowma.");

}
