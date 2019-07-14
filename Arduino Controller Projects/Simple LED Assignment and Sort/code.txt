#include <Adafruit_NeoPixel.h>

#define PIN            6
#define NUMPIXELS      24
int delayval = 47;

Adafruit_NeoPixel pixels = Adafruit_NeoPixel(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);

void setup() {
  pixels.begin(); // This initializes the NeoPixel library.
  pixels.setBrightness(255); //max brightness
  pixels.show();             // Initialize all pixels to 'off'
  
}

void loop() {
  // sets all pixels to a random color value
  for (int i = 0; i < NUMPIXELS; i++) { 
    uint32_t temp = pixels.Color(random(0,256),random(0,256),random(0,256));
    pixels.setPixelColor(i, temp);
    delay(delayval);
    pixels.show();
  }
  //loops through the LEDs doing a simple sort from largest value
  //to smallest value based on the 32 bit value returned from getPixelColor
  while(!check()){
  	for(int i = 0; i <(NUMPIXELS-1);i++){
    	if(pixels.getPixelColor(i) > pixels.getPixelColor(i+1)){
          	uint32_t temp1 = pixels.getPixelColor(i);
     		uint32_t temp2 = pixels.getPixelColor(i+1);
     		pixels.setPixelColor(i+1,temp1);
          	pixels.setPixelColor(i,temp2);
     		delay(delayval*2);
          	//delay(delayval*(NUMPIXELS - count));
     		pixels.show();
    	}
  	}
  }
  //clears the LEDs after sorting has been completed
  delay(1000);//wait a moment to see the result
  for (int i = 0; i < NUMPIXELS; i++) {
    uint32_t temp = pixels.Color(0,0,0);
    pixels.setPixelColor(i, temp);
    delay(delayval);
    pixels.show();
  }
}

//do a single run through the pixels to check if they are in order
bool check(){
  for(int i = 0; i <(NUMPIXELS-1);i++){
    	if(pixels.getPixelColor(i) > pixels.getPixelColor(i+1)){
			return false;
    	}
  	}
  return true;
}
