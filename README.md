# IoT Workshop VJEC

## App development for IOT
- [Visit Mit App inventor](http://appinventor.mit.edu/)


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


***Code***
```



```



## Other Websites to visit 
[IFTTT](https://ifttt.com/)


# Astrek Innovations 
Visit Us
[Website](https://www.astrekinnovations.com)
[Facebook](https://www.facebook.com/astrekinnovations/)

### Robin Thomas
[Website](https://www.robinkthomas.com)
[Facebook]https://(www.facebook.com/robinthomas82)
[Linkedin](https://www.linkedin.com/in/robinkanattuthomas/)
+919847115594
### Alex M Sunny
[Facebook](https://www.facebook.com/search4alex)
[Linkedin](https://www.linkedin.com/in/alex-m-sunny-b28b1bb9/)
+919048218562 
