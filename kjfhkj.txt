//CUPOFCOFFEEMAKESPBAHAPPY
#include <SoftwareSerial.h>    
SoftwareSerial GSM(11, 10); 

char phone_no[] = "+918529319782"; //aBHAY'SNO

#define bt_C  A1 

void setup() { 

  Serial.begin(9600);
  GSM.begin(9600);   

  pinMode(bt_C, INPUT_PULLUP); // button lagana hai 


  Serial.println("Initializing....");
  initModule("AT", "OK", 1000);              

}

void loop() {

  if (digitalRead (bt_C) == 0)
  {
    callUp(phone_no);
  }

  delay(5);
}


void callUp(char *number) {
  GSM.print("ATD + "); GSM.print(number); GSM.println(";"); 
  delay(20000);       
  GSM.println("ATH"); 
  delay(100);
}


void initModule(String cmd, char *res, int t) {
  while (1) {
    Serial.println(cmd);
    GSM.println(cmd);
    delay(100);
    while (GSM.available() > 0) {
      if (GSM.find(res)) {
        Serial.println(res);
        delay(t);
        return;
      } else {
        Serial.println("Error");
      }
    }
    delay(t);
  }
}
