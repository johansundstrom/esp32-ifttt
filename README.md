# ESP32-IFTTT
Generera händelser från ESP32 till IFTTT

## Planen
ESP32 svarar på en händelse (larm) med att skicka en notis

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
8. I steg 5(6) - Redigera rutans innehåll så att det står: ```{{OccurredAt}} - {{EventName}} field1 = {{Value1}}```. Klicka _Create action_
9. I steg 6(6) - _Finish_
10. Surfa till adressen ```https://ifttt.com/maker_webhooks```
11. Klicka _Documentation_ uppe till höger

https://github.com/johansundstrom/esp32-ifttt/blob/master/images/ifttt_01.jpg



## ESP32
1. Skapa en sketch från WiFiClient (ur ESP32's exempel)
2. 
