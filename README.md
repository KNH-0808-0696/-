#include <Wire.h>

#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27,16,2); 

int buzzer=2;

int led=8;

int lm35=A0;

int val;

float degree;

void setup()
{
  lcd.init();

lcd.backlight();

Serial.begin(9600);

pinMode(buzzer,OUTPUT);

pinMode(led,OUTPUT);

}

void loop()

{

val=analogRead(lm35);  

degree= (float)val*500/1024;
  
  Serial.print("Temperature = ");  

Serial.print(degree, 1);  

Serial.println(" C");

  if(degree > 28.0)

{

for (int i=1; i<30;i++) 

{

ledflashing(100);}

alarmsound();

lcddisplay();

}

else if
  
 (degree >=28.0 && degree <=32.0)

{

ledflashing(500);

noTone(buzzer);

lcddisplay();

}

else

{

ledflashing(1000);

noTone(buzzer);
  
  lcddisplay();
   }

}

void ledflashing(int k)

{

digitalWrite(led,HIGH);
  
  delay(k);
  
  digitalWrite(led,LOW);
  
  delay(k); 

}  


void alarmsound(){
  
  for(int i=700;i<=800;i++) 

{ 
  
  tone(buzzer,i);
  
  delay(15);

}

noTone(buzzer);
  
  delay(100); 
  
  for(int i=800;i>=700;i--) 

{

tone(buzzer,i);
  
  delay(15); 

}  
}

void lcddisplay()
{
    lcd.setCursor(2,0);
  
  lcd.print("Temperature");
  
  lcd.setCursor(2,1);
  
  lcd.print(degree,1);
  
  lcd.setCursor(6,1);
  
  lcd.print(" C");
  
  }  
 
