#include "WiFi.h"

#include <ThingSpeak.h>

#include <DHT11.h>

const char* ssid = "KT_WLAN_D074";

const char* password = "000000e742";

WiFiClient client;

unsigned long DHT_CHID = 1072386;

const char* DHT_WriteKey = "IBNVPY83MXHAGAWU";

DHT11 dht11(27);

void setup()

{

Serial.begin(115200);

initWiFi();

ThingSpeak.begin(client);

}

void initWiFi() {

WiFi.begin(ssid, password);

while (WiFi.status() != WL_CONNECTED) {

delay(500);

Serial.println(".");

}

}

void loop()

{

float temp, humi;

static bool state = true;

int result = dht11.read(humi, temp);

if (result == 0)

{

if(state == true)

{

ThingSpeak.writeField(DHT_CHID, 1, temp, DHT_WriteKey);

state = false;

}

else

{

ThingSpeak.writeField(DHT_CHID, 2, humi, DHT_WriteKey);

state = true;

}

}

delay(15000);

}
