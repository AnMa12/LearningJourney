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


# Imports
import tensorflow as tf
import serial

arduinoData = serial.Serial('com5',9600)

def change_color(persons_number):
    arduinoData.write(bytes([persons_number]))

# Object detection imports
from utils import backbone
from api import object_counting_api

if tf.__version__ < '1.4.0':
  raise ImportError('Please upgrade your tensorflow installation to v1.4.* or later!')


import cv2
import numpy as np

cap = cv2.VideoCapture(0)
print(cap.get(cv2.CAP_PROP_FPS))
cap.set(cv2.CAP_PROP_FPS, 50);
print(cap.get(cv2.CAP_PROP_FPS))
while True:
  ret, img = cap.read()
  # By default I use an "SSD with Mobilenet" model here. See the detection model zoo (https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md) for a list of other models that can be run out-of-the-box with varying speeds and accuracies.
  detection_graph, category_index = backbone.set_model('ssd_mobilenet_v1_coco_2017_11_17')
  is_color_recognition_enabled = 0
  result = object_counting_api.single_image_object_counting(img, detection_graph, category_index, is_color_recognition_enabled)
  # targeted objects counting
  if result == "":
    persons_number = 0
  else:
    persons_number = int(float(result[11:]))
  print(persons_number)
  change_color(persons_number)
  k = cv2.waitKey(10)& 0xff
  if k==27:
    break
cap.release()
cv2.destroyAllWindows()




#include <FastLED.h>
#define LED_PIN     7
#define NUM_LEDS    50
#define brightness 10

char serialData;

CRGB leds[NUM_LEDS];
void setup() {
  FastLED.addLeds<WS2812, LED_PIN, GRB>(leds, NUM_LEDS);
  FastLED.setBrightness(brightness);
  Serial.begin(9600);
  pinMode(LED_BUILTIN, OUTPUT);
  
}
void loop() {
  if(Serial.available() > 0) {
    serialData = Serial.read();
    Serial.print(serialData);

    if(serialData == 0) {
      for(int i = 0; i <= 20; i++) {
        leds[i] = CRGB(0, 0, 255);
        FastLED.show();
                delay(50);
      }
    }
    if(serialData == 1) {
      for(int i = 0; i <= 20; i++) {
        leds[i] = CRGB(255, 0, 0);
        FastLED.show();
                delay(50);
      }
    }
    if(serialData == 2) {
      for(int i = 0; i <= 20; i++) {
        leds[i] = CRGB(0, 255, 0);
        FastLED.show();
                delay(50);
      }
    }
    if(serialData == 3) {
      for(int i = 0; i <= 20; i++) {
        leds[i] = CRGB(255, 255, 0);
        FastLED.show();
                delay(50);
      }
    }
    if(serialData == 4) {
      for(int i = 0; i <= 20; i++) {
        leds[i] = CRGB(255, 255, 255);
        FastLED.show();
        delay(50);
      }
    }
  }
}
