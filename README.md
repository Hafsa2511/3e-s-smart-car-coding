 /*
                            *
                            * Project Name: 	3E's SMART CAR
                            * Author List: 		Shaikh Hafsa , Nitin suiwal , Sahil Sharma , Sumit Rana
                            * Filename: 		3e-s-smart-car-coding
                            * Functions: 		
                            * Global Variables:	<List of all the global variables defined in this file, None if no global
                            *			variables>
                            *
                            */
						
            #include <SPI.h>
#include <MFRC522.h>
#include <LiquidCrystal.h> // includes the LiquidCrystal Library
LiquidCrystal lcd(2, 3, 4, 5, 6,7); // Creates an LCD object. Parameters: (rs, enable, d4, d5, d6, d7)
 
#define SS_PIN 10
#define RST_PIN 9
MFRC522 mfrc522(SS_PIN, RST_PIN);   // Create MFRC522 instance.
 
void setup() 
{
  lcd.begin(16,2);
  lcd.print("Show your Card");
  
  // Look for new cards
  Serial.begin(9600);   // Initiate a serial communication
  SPI.begin();      // Initiate  SPI bus
  mfrc522.PCD_Init();   // Initiate MFRC522
  Serial.println("Approximate your card to the reader...");
  Serial.println();

}
void loop() 
{
  
 
  if ( ! mfrc522.PICC_IsNewCardPresent()) 
  {
    return;
  }
  
  // Select one of the cards
  if ( ! mfrc522.PICC_ReadCardSerial()) 
  {
    return;
  }
  
  //Show UID on serial monitor
  Serial.print("UID tag :");
  String content= "";
  byte letter;
  for (byte i = 0; i < mfrc522.uid.size; i++) 
  {
     Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
     Serial.print(mfrc522.uid.uidByte[i], HEX);
     content.concat(String(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " "));
     content.concat(String(mfrc522.uid.uidByte[i], HEX));
  }
  Serial.println();
  Serial.print("Message : ");
  content.toUpperCase();
  if (content.substring(1) == "39 2A B1 15"||content.substring(1) == "39 2A B1 16"||content.substring(1) == "39 2A c1 15") //change here the UID of the card/cards that you want to give access
  {
      Serial.println("Authorized access");
      Serial.println();
      
      lcd.clear();
      lcd.setCursor(0,0);
      lcd.print("   Authorized");
      lcd.display();
      lcd.setCursor(0,1);
      lcd.print("Opening Door....");
     
     
      
      delay(3000);
  }
 
 else   {

      Serial.println(" Access denied");
      lcd.clear();
      lcd.setCursor(0,0);
      lcd.print("  Access denied");
      lcd.setCursor(0,1);
      lcd.print("  Wrong Card! ");
      
      
      
      delay(3000);

      
  }
} 
