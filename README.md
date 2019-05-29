

## Basic Serial Communication


![Image](https://cdn.instructables.com/FIT/N9LV/IQ2KEGUO/FITN9LVIQ2KEGUO.LARGE.jpg)

***Code***
```
void setup()

{

pinMode(13, OUTPUT);

Serial.begin(9600);

while (!Serial);

Serial.println("Input 1 to Turn LED on and 2 to off");

}

void loop() {

if (Serial.available())

{

int state = Serial.parseInt();

if (state == 1)

{

digitalWrite(13, HIGH);

Serial.println("Command completed LED turned ON");

}

if (state == 2)

{

digitalWrite(13, LOW);

Serial.println("Command completed LED turned OFF");

}

}

}


```

## Basic PIR Motion Sensor
-  Assemble all the parts by following the schematics below.

![Image](https://i2.wp.com/randomnerdtutorials.com/wp-content/uploads/2014/08/Arduino-with-PIR-motion-sensor-schematics.jpg?ssl=1)

***Code***
```

int led = 13;                // the pin that the LED is atteched to
int sensor = 2;              // the pin that the sensor is atteched to
int state = LOW;             // by default, no motion detected
int val = 0;                 // variable to store the sensor status (value)

void setup() {
  pinMode(led, OUTPUT);      // initalize LED as an output
  pinMode(sensor, INPUT);    // initialize sensor as an input
  Serial.begin(9600);        // initialize serial
}

void loop(){
  val = digitalRead(sensor);   // read sensor value
  if (val == HIGH) {           // check if the sensor is HIGH
    digitalWrite(led, HIGH);   // turn LED ON
    delay(100);                // delay 100 milliseconds 
    
    if (state == LOW) {
      Serial.println("Motion detected!"); 
      state = HIGH;       // update variable state to HIGH
    }
  } 
  else {
      digitalWrite(led, LOW); // turn LED OFF
      delay(200);             // delay 200 milliseconds 
      
      if (state == HIGH){
        Serial.println("Motion stopped!");
        state = LOW;       // update variable state to LOW
    }
  }
}

```

## Basic Ultrasonic  Sensor
-  Assemble all the parts by following the schematics below.

![Image](https://cdn.instructables.com/F1T/3TQ2/H994BXMB/F1T3TQ2H994BXMB.LARGE.jpg?auto=webp&width=600&crop=3:2)

***Code***
```
#define trigPin 13
#define echoPin 12
#define led 11
#define led2 10

void setup() {
  Serial.begin (9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(led, OUTPUT);
  pinMode(led2, OUTPUT);
}

void loop() {
  long duration, distance;
  digitalWrite(trigPin, LOW);  // Added this line
  delayMicroseconds(2); // Added this line
  digitalWrite(trigPin, HIGH);
//  delayMicroseconds(1000); - Removed this line
  delayMicroseconds(10); // Added this line
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = (duration/2) / 29.1;
  if (distance < 4) {  // This is where the LED On/Off happens
    digitalWrite(led,HIGH); // When the Red condition is met, the Green LED should turn off
  digitalWrite(led2,LOW);
}
  else {
    digitalWrite(led,LOW);
    digitalWrite(led2,HIGH);
  }
  if (distance >= 200 || distance <= 0){
    Serial.println("Out of range");
  }
  else {
    Serial.print(distance);
    Serial.println(" cm");
  }
  delay(500);
}


```
## App development for IOT
- [Visit Mit App inventor](http://appinventor.mit.edu/)

# NODE MCU

## Basic Blinking Using Node MCU 
-  Open the preferences window from the Arduino IDE. Go to File >Preferences

![Image](https://maker.pro/storage/OAYqL3v/OAYqL3vUjwaZHA95fuU6Ln7RrIIWZtKLfMTGCGFY.jpeg)

-  Enter http://arduino.esp8266.com/stable/package_esp8266com_index.json into Additional Board Manager URLs field and click the “OK” button

![Image](https://maker.pro/storage/2aYeYxI/2aYeYxIaufaxwYdk0XGBnbwmdlyKx8Is04eN7zM8.jpeg)

-  Open boards manager. Go to: Tools > Board > Boards Manager…

![Image](https://maker.pro/storage/MzeFJ3o/MzeFJ3oxkkS7Jj7lSJ3s02jFh0z7HCJ4a62oRQwn.jpeg)

-  Scroll down, select the ESP8266 board menu and install “esp8266 platform”.

![Image](https://maker.pro/storage/vDFw2EX/vDFw2EXJvaA3vIlJLpbrYlqbMOHBE85tq1l1acwE.jpeg)

***Code***
```
#define LED D1            // Led in NodeMCU at pin GPIO16 (D0).
void setup() {
pinMode(LED, OUTPUT);    // LED pin as output.
}
void loop() {
digitalWrite(LED, HIGH);// turn the LED off.(Note that LOW is the voltage level but actually 
                        //the LED is on; this is because it is acive low on the ESP8266.
delay(1000);            // wait for 1 second.
digitalWrite(LED, LOW); // turn the LED on.
delay(1000); // wait for 1 second.
}

```


##  Blinking Using Internet
-  Connect Led to D7 of Node MCU

-  Enter **SSID** and your **Password** of your network in the code

-  Open Serial Monitor and Note down the IP address shown.

-  Type the IP adress in the browser of Mobile /Computer


***Code***
```
#include <ESP8266WiFi.h>
 
const char* ssid = "esp8266"; //ssid
const char* password = "12345678"; //pw of network
 
int ledPin = 13; // GPIO13---D7 of NodeMCU
WiFiServer server(80);
 
void setup() {
  Serial.begin(115200);
  delay(10);
 
  pinMode(ledPin, OUTPUT);
  digitalWrite(ledPin, LOW);
 
  // Connect to WiFi network
  Serial.println();
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);
 
  WiFi.begin(ssid, password);
 
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("WiFi connected");
 
  // Start the server
  server.begin();
  Serial.println("Server started");
 
  // Print the IP address
  Serial.print("Use this URL to connect: ");
  Serial.print("http://");
  Serial.print(WiFi.localIP());
  Serial.println("/");
 
}
 
void loop() {
  // Check if a client has connected
  WiFiClient client = server.available();
  if (!client) {
    return;
  }
 
  // Wait until the client sends some data
  Serial.println("new client");
  while(!client.available()){
    delay(1);
  }
 
  // Read the first line of the request
  String request = client.readStringUntil('\r');
  Serial.println(request);
  client.flush();
 
  // Match the request
 
  int value = LOW;
  if (request.indexOf("/LED=ON") != -1)  {
    digitalWrite(ledPin, HIGH);
    value = HIGH;
  }
  if (request.indexOf("/LED=OFF") != -1)  {
    digitalWrite(ledPin, LOW);
    value = LOW;
  }
 
// Set ledPin according to the request
//digitalWrite(ledPin, value);
 
  // Return the response
  client.println("HTTP/1.1 200 OK");
  client.println("Content-Type: text/html");
  client.println(""); //  do not forget this one
  client.println("<!DOCTYPE HTML>");
  client.println("<html>");
 
  client.print("Led is now: ");
 
  if(value == HIGH) {
    client.print("On");
  } else {
    client.print("Off");
  }
  client.println("<br><br>");
  client.println("<a href=\"/LED=ON\"\"><button>On </button></a>");
  client.println("<a href=\"/LED=OFF\"\"><button>Off </button></a><br />");  
  client.println("</html>");
 
  delay(1);
  Serial.println("Client disonnected");
  Serial.println("");
 
}


```

##  Connecting sensor to IoT platform
-  Go to Thinkspeak and make account [Thinkspeak](https://thingspeak.com/)

-  Enter **SSID** and your **Password** of your network in the code

-  Open Serial Monitor and Note down the IP address shown.

-  Type the IP adress in the browser of Mobile /Computer



# Raspberry Pi
##  Raspberry Pi Headless Setup
-  Refer this link to connect Respberry Pi in headless setup [Click here](https://hackernoon.com/raspberry-pi-headless-install-462ccabd75d0)


##  Raspberry Pi GPIO LED
![Image](https://cdn.sparkfun.com/r/600-600/assets/learn_tutorials/4/2/4/header_pinout.jpg)


Connect the circuit:

-  Use a jumper wire to connect the ground ( Pin 3) of GPIO to rail marked in blue on the breadboard.
-  Connect the LED with the cathode in the same row as the resistor. Insert the anode in the adjacent row.
-  Use another jumper cable to connect the GPIO Pin 21 ( 3.3 V) in the same row as the anode of LED.

***Code***
```
import RPi.GPIO as GPIO
import time 
GPIO.setmode(GPIO.BCM) 
GPIO.setwarnings(False) 
GPIO.setup(21,GPIO.OUT) 
While True:
   print "LED on" 
   GPIO.output(21,GPIO.HIGH) 
   time.sleep(10) 
   print "LED off" 
   GPIO.output(21,GPIO.LOW)

```

##  Raspberry Pi Camera


Open Python 3 from the main menu:

Open Python 3

Open a new file and save it as camera.py. It’s important that you do not save it as picamera.py.

Enter the following code:
***Code***
```
from picamera import PiCamera
from time import sleep

camera = PiCamera()

camera.start_preview()
sleep(10)
camera.stop_preview()
```
Save with Ctrl + S and run with F5. The camera preview should be shown for 10 seconds, and then close. Move the camera around to preview what the camera sees.

The live camera preview should fill the screen like so:

Image preview

Note that the camera preview only works when a monitor is connected to the Pi, so remote access (such as SSH and VNC) will not allow you to see the camera preview

If your preview was upside-down, you can rotate it with the following code:
***Code***
```
camera.rotation = 180
camera.start_preview()
sleep(10)
camera.stop_preview()
```
You can rotate the image by 90, 180, or 270 degrees, or you can set it to 0 to reset.

You can alter the transparency of the camera preview by setting an alpha level:
***Code***
```
from picamera import PiCamera
from time import sleep

camera = PiCamera()

camera.start_preview(alpha=200)
sleep(10)
camera.stop_preview()
```
alpha can be any value between 0 and 255.

##  Raspberry Pi Camera Terminal Commands 

Basic usage of raspistill
-  With the camera module connected and enabled, enter the following command in the Terminal to take a picture:

```
raspistill -o cam.jpg

```

-  Upside-down photo

-  In this example the camera has been positioned upside-down. If the camera is placed in this position, the image must be flipped to appear the right way up.

-  Vertical Flip & Horizontal Flip
-  With the camera placed upside-down, the image must be rotated 180° to be displayed correctly. The way to correct for this is to apply both a vertical and a horizontal flip by passing in the -vf and -hf flags:

```
raspistill -vf -hf -o cam2.jpg
```
-  Vertical and horizontal flipped photo

-  Now the photo has been captured correctly.

 Resolution
-  The camera module takes pictures at a resolution of 2592 x 1944 which is 5,038,848 pixels or 5 megapixels.

File size
-  A photo taken with the camera module will be around 2.4MB. This is about 425 photos per GB.

-  Taking 1 photo per minute would take up 1GB in about 7 hours. This is a rate of about 144MB per hour or 3.3GB per day.





## Other Websites to visit 
[IFTTT](https://ifttt.com/)
 

# Astrek Innovations 
Visit Us
[Website](https://www.astrekinnovations.com)
[Facebook](https://www.facebook.com/astrekinnovations/)

### Robin Thomas
[Website](https://www.robinkthomas.com)
[Facebook](https://www.facebook.com/robinthomas82)
[Linkedin](https://www.linkedin.com/in/robinkanattuthomas/)
+919847115594

