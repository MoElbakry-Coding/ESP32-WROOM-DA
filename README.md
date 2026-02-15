# ESP32-WROOM-DA
IDEASPARK_ESP32
#include <SPI.h>
#include <Adafruit_GFX.h>
#include <Adafruit_ST7789.h>
#include <DHT.h>

#define TFT_CS   15
#define TFT_DC   2
#define TFT_RST  4
#define TFT_BLK  32

#define DHTPIN   13
#define DHTTYPE  DHT11

Adafruit_ST7789 tft = Adafruit_ST7789(TFT_CS, TFT_DC, TFT_RST);
DHT dht(DHTPIN, DHTTYPE);

void setup() {

  Serial.begin(115200);

  pinMode(TFT_BLK, OUTPUT);
  digitalWrite(TFT_BLK, HIGH);

  tft.init(135, 240);
  tft.setRotation(1);
  tft.fillScreen(ST77XX_BLACK);

  dht.begin();
}

void loop() {

  float t = dht.readTemperature();
  float h = dht.readHumidity();

  if (isnan(t) || isnan(h)) {
    Serial.println("DHT read failed!");
    delay(2000);
    return;
  }



  tft.fillScreen(ST77XX_BLACK); // clear previous readings

  // Temperature on one line
  tft.setTextSize(2);            // adjust size to fit screen width
  tft.setTextColor(ST77XX_GREEN);
  tft.setCursor(10, 30);         // starting position
  tft.print("Temperature ");     // label
  tft.print(t);                  // value
  tft.print(" C");               // unit

  // Humidity on one line
  tft.setTextSize(2);
  tft.setTextColor(ST77XX_CYAN);
  tft.setCursor(10, 80);         // next line
  tft.print("Humidity ");        // label
  tft.print(h);                  // value
  tft.print(" %");               // unit

  delay(2000);  // DHT11 needs slow reading
}
