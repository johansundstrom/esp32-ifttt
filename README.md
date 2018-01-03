# ESP32-IFTTT
Generera händelser från ESP32 till IFTTT

## Planen
ESP32 svarar på en händelse (larm) med att skicka en notis till mobilen

## IFTTT
* If Thit Then That (IFTTT)
* Sambandscentral mellan alla applikationers API
* _This_ kan exempelvis vara...
  * en tid
  * en geografisk plats
  * ett villkor
  * en nyhet
  * osv...
* _That_ kan exempelvis vara 
  * en notis
  * ett mail
  * en ny cell i ett kalkylark
  * ett inlägg i sociala medier
* 
1. Skapa konto på IFTTT
2. Gå till _My Applets_
3. Klicka på _+this_
4. Välj _Webhooks_ (hette tidigare Maker) - Receive a web request
5. I steg 2(6) - Skapa _Event Name_ "ESP_says". _Create_
6. I steg 3(6) - Klicka på _+that_ och välj _Notifications_
7. I steg 4(6) - Klicka på blå rutan
8. I steg 5(6) - Redigera rutans innehåll så att det står: ```{{OccurredAt}} - {{EventName}}: field1 = {{Value1}}```. Klicka _Create action_
9. I steg 6(6) - _Finish_
10. Surfa till adressen ```https://ifttt.com/maker_webhooks```
11. Klicka _Documentation_ uppe till höger

<img src="https://github.com/johansundstrom/esp32-ifttt/blob/master/images/ifttt_01.jpg" width="400">

12. I rutan för {event} - skriv ```ESP_says```
13. I _value1_ skriv ett slumpmässigt värde
14. Installera appen IFTTT i telefonen, logga in
15. Klicka _Test It_
16. Kolla telefonen

## ESP32
Vi modifierar sketch'en från WiFiClient (ur ESP32's exempel) lite.

#### Initiering...
```c
#include <WiFi.h>

const char* ssid     = "XXXXXXXXXXX";
const char* password = "YYYYYYYYYYY";

const int   httpPort = 80;
const char* host = "maker.ifttt.com";
const char* event = "ESP_says";
const char* key = "ZZZZZZZZZZZZZZZZZZZZZ";
```

#### Setup...
```c
void setup()
{
    Serial.begin(115200);
    delay(10);

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
    Serial.println("IP address: ");
    Serial.println(WiFi.localIP());
}
```

#### Loopen...

```c
void loop()
{
    delay(5000);

    Serial.print("connecting to ");
    Serial.println(host);

    // Use WiFiClient class to create TCP connections
    WiFiClient client;

    if (!client.connect(host, httpPort)) {
        Serial.println("connection failed");
        return;
    }

    // We now create a URI for the request
    String url = "/trigger/";
    url += event;
    url += "/with/key/";
    url += key;
    url += "?value1=Johan";

    Serial.print("Requesting URL: ");
    Serial.println(url);

    // This will send the request to the server
    client.print(String("GET ") + url + " HTTP/1.1\r\n" +
                 "Host: " + host + "\r\n" +
                 "Connection: close\r\n\r\n");
                 
    unsigned long timeout = millis();
    while (client.available() == 0) {
        if (millis() - timeout > 5000) {
            Serial.println(">>> Client Timeout !");
            client.stop();
            return;
        }
    }

    // Read all the lines of the reply from server and print them to Serial
    while(client.available()) {
        String line = client.readStringUntil('\r');
        Serial.print(line);
    }

    Serial.println();
    Serial.println("closing connection");
}
```

Ladda upp och testa

## Use Case
Fundera över...
* Vad kan trigga?
* Vad kan hända?
* Vad ska hända?

## OLED?

```c
#include <Wire.h>
#include "SSD1306.h" 

SSD1306  display(0x3c, 21, 22);

void setup() {
  display.init();
  //display.flipScreenVertically();
  display.drawString(0, 0, "Johan Sundström");
  display.display();
}

void loop() {
}
```
