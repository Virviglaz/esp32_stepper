# ESP32 simple stepper driver

Description: This driver handles the stepper motor control. It can work in main thread using waiting calls or run as a background task pinned to the less used core.

## Code example
~~~cpp
#include "esp_stepper.h"

#define CLK_PIN   GPIO_NUM_32
#define DIR_PIN   GPIO_NUM_25

const int SPEED_IN_STEPS_PER_SECOND = 3200;
const int ACCELERATION_IN_STEPS_PER_SECOND = 3200;
const int DECELERATION_IN_STEPS_PER_SECOND = 3200;

stepper motor(CLK_PIN,
      DIR_PIN, SPEED_IN_STEPS_PER_SECOND,
      ACCELERATION_IN_STEPS_PER_SECOND,
      DECELERATION_IN_STEPS_PER_SECOND);

void setup() {
    
}

void loop() {
  /* rotate CW */
  motor.move(3200);
  printf("1. Done\n");

  /* rotate CCW */
  motor.move(-3200);
  printf("2. Done\n");
}
~~~
### Additional information
In this example the next motor.move() call is a waiting call. It detects that previous movement is not done and keeps controlling the motor until the movement is finished.
In order to control the motor using non-waiting calls, you should specify which CPU you want to use and which queue size you need to keep your movement data.
Queue size equal to 1 will make a second motor.move() waiting. A bigger queue size will simply be a memory waste. If you want program for example 5 movement and keep executing your code furder you need to use queue size = 5.
There is no checking for stop functionality implemented. Also, it is not checking for the next segment to properly change the speed. Thus it's not suitable for the CNC control.
 Only for some simple applications.
