# TURNING ON A MOTOR USING FINGERPRINT SENSOR

# 1.ABSTRACT:
This project “ designed and implementation of turning ON motor by using finger print sensor”. this is designed to work on switching motor for raising strong security of system and this system is reliable so, is composed by three main part that are sensing part, processing part, and indicating part. There is also Arduino contain microchip used with finger print, the system it can only be opened when an authorized user is present. 
 This work is focused on protecting system from unauthorized users and to prevent the   theft. Using biometric fingerprint security system only authorized persons can start the motor. This makes the motor protected. Methods/Analysis: The security system usage is increasing and is necessary all over the world. Usage of biometrics like fingerprint is used widely and is common in factories, buildings, schools and colleges and many more applications. Findings: This project deals with the protection of the motor which leads to the development of the antitheft system in a motor starting using AT mega 328. A fingerprint sensor which is kept near the system is used to sense the fingerprint. Fingerprint sensor data reading obtained in the AT mega 328 which is analyzed with the pre-assigned data. Identifying the person as the system owner or an authorized fingerprint user who can take control of the system, the motor system starts. If it is an intruder, a motor never starts. Improvement: Other security systems can be hacked, whereas in this case fingerprint is being used as the key which is unique for each person and therefore gives improved security.

# 2. PROBLEM STATEMENTS
After noticing that there is a problem of security along system and luck of confidence to start system (turn on) most people and industries need to turn on their system, the use of keys, password and other switches introduced, but their meet with the issues or challenges that are appear in such situation.
Firstly, same need to   secure their system which cannot be easy and not confidential using password, keys and other switching system.
Secondary, according to the loss of security pattern or password and keys of starting system especial system includes motor.                                           After seeing those problems, I through about how to solve those problems by using fingerprint system. 

# 3. BLOCK DIAGRAM
 ![image](https://user-images.githubusercontent.com/104354264/165166405-9d4a0946-0d97-4c70-8d13-ffa77ba0eefe.png)

Sensing part include finger print sensor:
Is a type of electronics security system that uses fingerprints for biometric authentication to grant a user access to information or to approve transaction Fingerprint scanner work by capturing the pattern of the ridges and valley on finger, The information is then processed by the device’s pattern analysis / matching software, which compares it to the list of registered finger print on file Successful matching, means an identification has been verified, thereby gradient access.  Hence, sensing part contain fingerprint sensor module which is used for fingerprint detection and is more accessible as well easy to use in project .by using this we can make registration, fingerprint correction, such and compare those modules are inbuilt with flash memory that store fingerprint.
# Processing part:
The Arduino Uno is a microcontroller board based on the ATmega328.The Uno board functioning is different from all other boards in that it does not use the FTDI USB to serial driver chip. Instead, the Atmega328 is programmed as a USB to serial converter. The ATmega328 is a low power CMOS 8-bit microcontroller based on the AVR enhanced RISC architecture structure.
The fingerprints of only the authorized persons are enrolled for further verification into the microcontroller. The microcontroller controls the fingerprint sensor when an authorized person is scanned it allows the relay to contact and supply power to a motor. Then a moto can be turned on. If an unauthorized person is scanned it does not make the relay to turn on. So, no current flows to a motor and it cannot be turned on. 
# Indicating part:                                                                                                                                                                                                                             
This part of block diagram is composed by relay and motor, relay is energized by Arduino with low current, after relay energized switch high current to the motor, a motor run when finger print enroll authorized by indicating the action starting form in put(fingerprint) to processor end by turning motor.  



# 4.CIRCUIT DIAGRAM
 
![image](https://user-images.githubusercontent.com/104354264/165166505-9aaa661a-28b0-45f5-80c7-612cf5f833d9.png)


# Source code:

#include <Adafruit_Fingerprint.h>

#include <SoftwareSerial.h>

SoftwareSerial mySerial(2, 3);

Adafruit_Fingerprint finger = Adafruit_Fingerprint(&mySerial);

void setup()  
{
  
  pinMode(9,OUTPUT);
	
  Serial.begin(9600);
	
  while (!Serial);  
	
  delay(100);
	
  Serial.println("\n\nAdafruit finger detect test");
	
  finger.begin(57600);
  
  if (finger.verifyPassword()) {
	
    Serial.println("Found fingerprint sensor!");
		
  } else {
	
    Serial.println("Did not find fingerprint sensor :(");
		
    while (1) { delay(1); }
		
  }

  finger.getTemplateCount();
	
  Serial.print("Sensor contains ");
	
  Serial.print(finger.templateCount); 
	
  Serial.println(" templates");
	
  Serial.println("Waiting for valid finger...");
	
}

void loop()

{

  getFingerprintIDez();
	
  delay(5);           
	
digitalWrite(9,HIGH);

}

uint8_t getFingerprintID() {

  uint8_t p = finger.getImage();
	
  switch (p) {
	
    case FINGERPRINT_OK:
		
      Serial.println("Image taken");
			
      break;
			
    case FINGERPRINT_NOFINGER:
		
      Serial.println("No finger detected");
			
      return p;
			
    case FINGERPRINT_PACKETRECIEVEERR:
		
      Serial.println("Communication error");

      return p;
			
    case FINGERPRINT_IMAGEFAIL:
		
      Serial.println("Imaging error");
			
      return p;
			
    default:
		
      Serial.println("Unknown error");
			
      return p;
			
  }
  p = finger.image2Tz();
	
  switch (p) {
	
    case FINGERPRINT_OK:
		
      Serial.println("Image converted");
			
      break;
			
    case FINGERPRINT_IMAGEMESS:
		
      Serial.println("Image too messy");
			
      return p;
			
    case FINGERPRINT_PACKETRECIEVEERR:
		
      Serial.println("Communication error");
			
      return p;
			
    case FINGERPRINT_FEATUREFAIL:
		
      Serial.println("Could not find fingerprint features");
			
      return p;
			
    case FINGERPRINT_INVALIDIMAGE:
		
      Serial.println("Could not find fingerprint features");
			
      return p;
			
    default:
		
      Serial.println("Unknown error");
			
      return p;
			
  }
  
  p = finger.fingerFastSearch();
	
  if (p == FINGERPRINT_OK) {
	
    Serial.println("Found a print match!");
		
  } else if (p == FINGERPRINT_PACKETRECIEVEERR) {
	
    Serial.println("Communication error");
		
    return p;
		
  } else if (p == FINGERPRINT_NOTFOUND) {
	
    Serial.println("Did not find a match");
		
    return p;
		
  } else {
	
    Serial.println("Unknown error");
		
    return p;
  }   
  
  Serial.print("Found ID #"); Serial.print(finger.fingerID); 
	
  Serial.print(" with confidence of "); Serial.println(finger.confidence); 

  return finger.fingerID;
}

int getFingerprintIDez() {

  uint8_t p = finger.getImage();
	
  if (p != FINGERPRINT_OK)  return -1;

  p = finger.image2Tz();
	
  if (p != FINGERPRINT_OK)  return -1;

  p = finger.fingerFastSearch();
	
  if (p != FINGERPRINT_OK)  return -1;
	
digitalWrite(9,LOW);

  delay(20000);
	
  Serial.print("Found ID #"); Serial.print(finger.fingerID); 
	
  Serial.print(" with confidence of "); Serial.println(finger.confidence);
	
  return finger.fingerID; 
}


