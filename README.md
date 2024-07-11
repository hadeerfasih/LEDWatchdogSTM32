# LEDWatchdogATmega328P
## Overview
This project implements an LED blinking functionality with watchdog supervision capability on an ATmega328P microcontroller. The LED blinking is managed through two software components: LEDMgr and GPIO, while the watchdog supervision is handled by WDGDrv and WDGM components. The project aims to ensure reliable LED operation by utilizing a watchdog timer to reset the system in case of malfunction.

## Components
### GPIO
GPIO_Init: Initializes the GPIO configuration for the pin used to control the LED.
GPIO_Write: Writes a specific value (0 or 1) to the pin.
### LEDMgr
LED_Init: Initializes the internal variables of the LED component.
LED_Manage: Manages the LED blinking actions using the GPIO_Write function. It is called every 10ms to handle the LED blinking periodicity of 500ms for each stage.
### WDGDrv
WDGDrv_Init: Configures the watchdog driver to support the following features:
Sets the maximum timeout value to 64ms.
Activates the watchdog.
### WDGM
WDGM_Init: Initializes the internal variables of the WDGM.
WDGM_MainFunction: Checks the number of calls to LED_Manage within a 100ms period to ensure they are between 8 and 12. It is called every 20ms.
WDGM_ProvideSupervisionStatus: Provides the status of the LEDM entity to the watchdog driver.
WDGM_AlivenessIndication: Detects the correct timing of the call from the LED_Manage function.

## Simulation Scenarios
A. Positive Scenario
Verifies the periodicity of LED blinking.
Checks calls to LEDM_Manage, WDGM_MainFunction, and refreshment of WDGDrv.
Timing evidence is provided by toggling test pins on the oscilloscope.
B. Negative Scenario 1
Comments out the call to WDGM_MainFunction.
Verifies that the watchdog reset occurs after 50ms.
C. Negative Scenario 2
Comments out the call to WDGM_AlivenessIndication from LEDM_Manage.
Verifies that the watchdog reset occurs after 100ms.
D. Negative Scenario 3
Changes the periodicity of calls to LEDM_Manage to every 5ms.
Verifies that the watchdog reset occurs after 100ms.

## Requirements
### ATmega328P microcontroller.
### Development environment set up for ATmega328P (e.g., Atmel Studio, Arduino IDE).
### Oscilloscope for timing verification.
