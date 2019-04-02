# LearningJourney
#include <FastLED.h>
#define LED_PIN     7
#define NUM_LEDS    50
#define brightness 10
CRGB leds[NUM_LEDS];
void setup() {
  FastLED.addLeds<WS2812, LED_PIN, GRB>(leds, NUM_LEDS);
  FastLED.setBrightness(brightness);
  
}
void loop() {

  for(int i = 0; i <= 20; i++) {
    for(int j = 0; j <= 20; j++) {
      if(j <= i + 2 && j >= i - 1) {
        //leds[j] = CRGB(0, 119, 179);
        leds[j] = CRGB(255, 255, 0);
          FastLED.show();
      } else {
        //leds[j] = CRGB(102, 204, 255);
        leds[j] = CRGB(102, 255, 51);
          FastLED.show();
      }
    }
    delay(5);
  }
}
