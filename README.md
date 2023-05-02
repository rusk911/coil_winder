# coil_winder
Guitar pickups winder

Components:
1. Arduino UNO R3 (ATmega328P)
2. TFT touch display ILI9488 with Arduino UNO compatibile direct mount header for setting coil parameters and speed
3. Bipolar stepper motor for main motor strong enough for rotating a coil. (I used 23HS16-0884S)
4. Stepper motor driver able to power your motor. (I used DM320T)
5. Any CD/DVD drive for parts. I used mini stepper with linear screw from some DVD. It has usually about 0.1A/phase and 18 degrees full step and 3mm per revolution screw.
6. Stepper motor driver for mini stepper, for example A4988. (I used tb6600)
7. A button for start/stop winding.
8. Power supply with 5V and 12V at least 1.5A. 
9. Wires, screws, some materials for assembly, hands.

Instructions:

Install Arduino IDE. Inside Arduino IDE install libraries Adafruit_GFX, MCUFRIEND_kbv and TouchScreen. Go to menu File -> Examples -> MCUFRIEND_kbv -> TouchScreen_Calibr_native. Connect your Arduino with attached display to USB port and upload the sketch. Switch Arduino IDE to serial monitor (Ctrl+Shift+M) and calibrate your TouchScreen touching "+" symbols. After that copy suggested lines from serial monitor and replace lines 10 and 11 in coil_winder.ino. This is the only adjustments you need to make in the code as it depends on TouchScreen parameters, which may vary between samples.

On TFT display cut off 4 pins marked for SD card reader, it's not used in this project. We'll use those pins for steppers and button.
Connect a button with a pull up resistor to pin 10.

You will need to make an adjustable end-stop, connected in paralel with main button to same pin 10. On startup DVD stepper motor starts moving left to end stop, then right to offset position, then it's ready to work. When you edit offset on touch screen, new homing cycle performed and DVD stepper motor goes to new start position. After coil is wound to desired turns number, a homing cycle performed. For testing you can use your button instead, it works same for homing cycle. 

When motor is moving, GUI is off, no touch quering performed because it's too slow. That's why I added a hardware button.

Main motor driver pulse contact connected to pin 11. As we don't need direction, you can leave dir contact on the driver disconnected or connect to 5V if driver wants it to rotate. If motor moves in wrong direction, connect motor wires opposite or use pull up resistor between dir contact and ground. Some drivers want a logical dir signal to rotate, some not. I used it to free a pin for hardwire button.

DVD stepper motor driver dir contact connects to pin 12.
DVD stepper motor driver pulse contact connects to pin 13.

Set main motor driver to 1/2 microstepping, which means 400 steps per rotation. It's hardcoded value, motor moves smooth enough with this settings.
Set DVD motor driver to 1/16 microstepping, which means about 0.01 per step, which is enough precision for positioning 0.056 gauge wire.

I have measured speed with different delay between steps. Seems like maximum speed is about 785 rev/min without any added delay, which means it takes about 191 microseconds per step for calculations. It should work for any Arduino UNO board and any drivers. If not, set lower speed using touch screen GUI or buy better motor and driver. 
