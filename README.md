// Reads the value of the Grove - Temperature Sensor, converts it to a Celsius temperature,
// Then converts to Fahrenheit as well and prints it to the LCD Screen.

#include <Wire.h>
#include "rgb_lcd.h"

const int enabler = 6;
const int pinTemp = A0;
int read_value;
rgb_lcd lcd;
 
 // This value is a property of the thermistor used in the Grove - Temperature Sensor,
// and used to convert from the analog value it measures and a temperature value.
const int B = 3975;
 
void setup()
{
  pinMode(enabler,INPUT);
  Serial.begin(9600);
  lcd.begin(16,2);                                                                         

}

void loop()
{ 
  lcd.display(); //enables lcd  
  //lcd.setRGB(255,255,255);
  read_value = digitalRead(enabler);
   if (read_value == HIGH)
   {
    // Get the (raw) value of the temperature sensor.
    int val = analogRead(pinTemp);

    // Determine the current resistance of the thermistor based on the sensor value.
    float resistance = (float)(1023-val)*10000/val;

    // Calculate the temperature based on the resistance value.
    float temperature = 1/(log(resistance/10000)/B+1/298.15)-273.15;
    int fahr =(temperature * 2) + 30;

    // Print the temperature to the serial console.
    lcd.print("Celsius: ");
    lcd.println(temperature);
    lcd.print("Fahrenheit: ");
    lcd.println(fahr);
    // Wait one second between measurements.
    delay(1000);
    lcd.clear();
   }
   else 
 {
  lcd.print("Standby");
 delay(1000); 
 lcd.clear();
 }
}
