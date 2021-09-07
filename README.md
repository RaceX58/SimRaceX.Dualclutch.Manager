# RaceX Bitepoint Manager

This SimHub plugin allows you to store the bitepoint for each car.
When the plugin detects a car change it automatically load the corresponding dashboard.

How to install the plugin ?

Download the latest release from the Releases section :
https://github.com/RaceX58/RaceX.Dashboard.Manager/releases

Extract the zip file in your SimHub program directory.
Lauch SimHub and activate the plugin.


## Prerequires

In order to use the plugin you need to implement your own logic to the Arduino sketch controlling the clucth paddles

The plugin exposes a Simhub property called [RaceXBitepointManagerPlugin.CurrentBitePoint]

You need to send the value of this property to the Arduino sketch.

This can be done by using the "Custom protocol" field of the Arduino object in SimHub

![send_custom_property](https://user-images.githubusercontent.com/24957190/132358703-4691979d-5cbf-4b02-9d78-3437e4beccc3.JPG)

Then you need to create a receiver in the Arduino sketch.

Here is an example using SHCustomProtocol.h :
```
#ifndef __SHCUSTOMPROTOCOL_H__
#define __SHCUSTOMPROTOCOL_H__

#include <Arduino.h>

class SHCustomProtocol {
private:

public:

  // Called when starting the arduino (setup method in main sketch)
	void setup() {
	}


	// Called when new data is coming from computer
	void read() {
            uint16_t bitePointValue = FlowSerialReadStringUntil('\n').toInt();
            PaddleClutchManager.setBitePointPercent(bitePointValue);
	}

	// Called once per arduino loop, timing can't be predicted, 
	// but it's called between each command sent to the arduino
	void loop() {   
	}

	// Called once between each byte read on arduino,
	// THIS IS A CRITICAL PATH :
	// AVOID ANY TIME CONSUMING ROUTINES !!!
	// PREFER READ OR LOOP METHOS AS MUCH AS POSSIBLE
	// AVOID ANY INTERRUPTS DISABLE (serial data would be lost!!!)
	void idle() {
	}
};

#endif
```

## How to use the plugin ?

Once you have implemented your Ardunino logic you can start to use the plugin.

- Set the bitepoint change steps (%)
- Define some buttons or a rotary for increasing and decreasing the bitepoint
- Load a game and jump to the car
- Adjust the bitepoint

The plugin will store the value and automatically load it each time you jump to the car again

![bitepoint_config](https://user-images.githubusercontent.com/24957190/132359819-72256a96-6cfe-49ca-9771-f19445224870.JPG)

## What next ?

Using the declared property and the associated event you can display the bite point value on multiple devices (dashboard, 7 segments displays, etc...)

Please report feedback.



Have Fun !

RaceX



