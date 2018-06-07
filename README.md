# Matej
Differential Temp Controller
program
#include <OneWire.h>
#include <TFT_HX8357.h> // LCD knjiznica
TFT_HX8357 tft = TFT_HX8357();       // Invoke custom library
#include <DallasTemperature.h>
#define ONE_WIRE_BUS 10 // na 10 priklopimo senzorje (4k7 upor)
OneWire oneWire(ONE_WIRE_BUS);
DallasTemperature sensors(&oneWire); //Pass our oneWire reference to Dallas Temperature.

int tankmin = 30;   // min temperatura . kolektor mora imeti za diff višjo, da se vklopi crpalka.

int izkl = 4; // crpalko ustavimo ko je diferncna temperatura med kolektorjem in tankom

int diff = 6; // crplako zazenemo ko je diferncna temperatura med kolektorjem in tankom vecja

int pump = 2;  //dolocimo izhod rele za crpalko priklopimo  na pin 2

int pumpstate; // stanje crpalke - izpis

void setup ()
{
  tft.init();
  tft.setRotation(1);
  tft.fillScreen(TFT_BLACK); // Clear screen to navy background
  Serial.begin(9600);
  sensors.begin();
  delay(1000);
  pinMode (pump, OUTPUT); //set pins to output
}

void loop ()
{
delay(1000);
sensors.requestTemperatures(); // posljes komando za citanje temperature
float tank =(sensors.getTempCByIndex(1));
float collector =(sensors.getTempCByIndex(0)); // citanje temperature

tft.setTextSize(3);
tft.setCursor (100,20);
tft.print ("SOLAR KONTROLER");  // 

tft.setTextSize(2);
tft.setCursor (100,80);
tft.print ("Temperatura kolektorja = "); // izpis za kolektor
tft.setCursor (400,80);
tft.print (collector); 
tft.print ("");

tft.setCursor (100,120);
tft.print ("Temeperatura bojlerja = "); // izpis za tank
tft.setCursor (400,120);
tft.print (tank);
tft.print ("");

tft.setCursor (100,160); 
tft.print ("Stanje crpalke = "); // izpis za crpalko
tft.setCursor (400,160);
tft.print (pumpstate);

tft.setCursor (100,200); 
tft.print ("Diferenca vklopa T = ");  // izpis za diferenco
tft.setCursor (400,200);
tft.print (diff);

tft.setCursor (100,240); 
tft.print ("Diferenca izklopa T = ");  // izpis za diferenco
tft.setCursor (400,240);
tft.print (izkl);

tft.setCursor (120,300);
tft.print ("PRO1 - Matej Zamuda");  // izpis


if ( ( (collector - diff) > tank ) && (collector >tankmin))

{digitalWrite (pump, HIGH);pumpstate =1; }


else 
if
((collector - izkl)< tank)
{digitalWrite (pump,LOW); pumpstate = 0; } 
delay (1000);
}

