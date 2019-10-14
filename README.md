# MilliTimer
A software timer class for use in Arduino environments based on the use of the built in millis() function.

## Instructions for use:
1. Place `MilliTimer.h` and `MilliTimer.cpp` in the folder with your `.ino` file.
2. Add `#include "MilliTimer.h"` to the top of your `.ino` file.
3. Create a timer instance as a global variable (by placing it above the `setup()` function).
4. Check or update the timer etc in the `loop()` function.

## Example code (non-blocking version of Blink.ino):

```
// Example program to show how to blink the built-in LED with non-blocking code by using an instance of the MilliTimer class.
#include "MilliTimer.h"

MilliTimer timer(500); // Creates a timer that will time out after 500 milliseconds.
bool ledState = false;    // Bool to store state of built-in LED.

void setup(){
  pinMode(LED_BUILTIN, OUTPUT); // Set built-in LED pin to output. LED_BUILTIN = 13 for standard Arduinos.
  digitalWrite(LED_BUILTIN, ledState);
}

void loop(){
  if(timer.timedOut(true)){
    ledState = !ledState; // toggle the LED status
    digitalWrite(LED_BUILTIN, ledState);
  }
}  
```

## Example code (Time delay/lag switch):

```
// Press the button and the LED will turn on. After a delay of 1 minute, the LED will switch off and stay off until the button is pressed again.
#include "MilliTimer.h"

MilliTimer timer(60000); // Creates a timer that will time out after 500 milliseconds.
bool ledState = false;    // Bool to store state of built-in LED.

void setup(){
  pinMode(10, INPUT_HIGH); // Attach momentary press button switch between pin 10 and 0V.
  pinMode(LED_BUILTIN, OUTPUT); // Set built-in LED pin to output. LED_BUILTIN = 13 for standard Arduinos.
  digitalWrite(LED_BUILTIN, ledState);
}

void loop(){
  // If button is pressed, switch on the LED and reset timer to 0 ms.
  if(digitalRead(10)){
    digitalWrite(LED_BUILTIN, HIGH);
    ledState = true;
    timer.reset();
  }
  
  // Switch off the LED if the timer has timed out.
  // By using the bool ledState, we only need to check to see if the light needs to be switched off if the light is on (timer not timed out).
  if(ledState && timer.timedOut()){
    digitalWrite(LED_BUILTIN, LOW);
    ledState = false;
  }
}  
```
