
//******CONFIGURACION PARA EL MOTOR PASO A PASO ****************************************************************************
#define PPR 400 //Pulsos por revolucion                                                                                    *
#define STEP_DELAY 4000 // tiempo en las cuales se ejecutan los pulsos 400 es el tiempo mas rapido para dar los pasos      *
const byte stepPin=9;// Numero del pin al que se conecta el step del driver                                                *                                                         
const byte directionPin=8;// Numero del pin al que se conecta el pin de direccion del driver                               *
bool running = true;   // Variable para controlar la activacion de la secuencia                                            *
//***************************************************************************************************************************
//******CONFIGURACION DEL SENSOR AS5600**************************************************************************************
#include <Wire.h>//                                                                                                           *
#include <AS5600.h>//                                                                                                         *
AMS_5600 ams5600;//                                                                                                           *                                                                                                           
#define DIR_PIN 2//                                                                                                           *
int angle_in = 0;//                                                                                                           *
//***************************************************************************************************************************
void setup()
{ 
    pinMode(DIR_PIN, OUTPUT);
    Wire.begin();
    Serial.begin(9600);
    pinMode(stepPin, OUTPUT);
    pinMode(directionPin, OUTPUT);
}
void loop()
{
  

    
    giro(200,true); // 200 pasos una vuelta completa
    detectar_angulo1();
    compara();
 
  
    
}

void giro(int grado, bool runnig){
 delay(3000);
 digitalWrite(directionPin,HIGH);
  if(running==true){
    for (int i = 0; i < grado; i++)
    {
        digitalWrite(stepPin,LOW);
        delayMicroseconds(STEP_DELAY);
        digitalWrite(stepPin,HIGH);
        delayMicroseconds(STEP_DELAY);
    }
    running=false;
    Serial.print(running);
  }
}

 int Angle() {
  digitalWrite(DIR_PIN, HIGH);
  int in;
  in = map(ams5600.getRawAngle(),0,4095,0,360);
  return in;
}

 void detectar_angulo1() {
  if(running=true){
   if (ams5600.detectMagnet() == 1 ) {
    if (angle_in != Angle()) {
      angle_in = Angle();
      Serial.println(angle_in);
    }
   }  
   running=false;
  }
 }

 void compara(){
   if (angle_in>=200)
   {
      running=true;
     giro(100,true);
   }

 }
