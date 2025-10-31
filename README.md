# Smart-Street-Light-System-with-IoT
The Smart Street Light System is designed to automatically control street lighting based on ambient light and motion detection, helping to conserve energy and improve road safety. The system uses sensors (LDR and PIR) to intelligently switch or dim lights depending on the environment and presence of vehicles or pedestrians.

Features
 Automatic ON/OFF based on day and night using LDR.
 Motion detection with PIR sensor to brighten lights only when movement is detected.
 Energy-saving dim mode (25% brightness during no motion).
 Solar-powered operation (optional).
 Wi-Fi control and data monitoring (optional with ESP8266).
 LCD Display to show status of lights and sensors (optional feature).

Components used:
1. Arduino UNO
2. Breadboard
3. LDR Sensor Module
4. PIR Sensor Module
5. I2C LCD 16x2 Display
6. LED (Street Light)
7. 220Ω Resistor
8. 9V/12V DC Power Supply
9. Jumper Wires

Code algorithm:
1. Start
2. Initialize Components
• Initialize I2C communication for LCD at address 0x27 (16×2
display).
• Set LDR_PIN = A0, PIR_PIN = 2, and LED_PIN = 9.
• Set pin modes:
o PIR_PIN → INPUT
o LED_PIN → OUTPUT
• Start serial communication at 9600 baud rate.
• Display a welcome message on the LCD (“SMART STREET
LIGHT SYSTEM”).
3. Display Calibration Message
• Show "System Ready" and "LDR Calibrated" on the LCD.
• Print LDR threshold details to the Serial Monitor.
4. Enter Main Loop (Repeat Continuously)
Step 4.1: Read Sensor Values
• Read LDR value from analog pin (0–1023).
• Read motion status from PIR sensor (0 = no motion, 1 =
motion).
Step 4.2: Update Motion Timer
• If motion is detected:
o Store current time using millis() in lastMotionTime.
o Display message “VEHICLE DETECTED – FULL
BRIGHTNESS” in Serial Monitor.
Step 4.3: Check Valid Motion
• Calculate time elapsed since the last motion.
31
• If less than 30 seconds (motionTimeout) → motion is still
valid (validMotion = true).
• Otherwise, motion is invalid (validMotion = false).
Step 4.4: Determine Light Condition Based on LDR
Compare the LDR value with thresholds:
Light Condition LDR
Range Action
Bright Day > 600 Call handleBrightDay()
Cloudy/Rainy 350–600 Call
handleRainyDay(validMotion)
Evening/Dark
Cloudy 200–350 Call handleEvening(validMotion)
Night < 200 Call
handleFullNight(validMotion)
5. Lighting Functions
(a) handleBrightDay()
• Turn OFF LED (brightness = 0).
• LCD: “BRIGHT DAY / LIGHTS: OFF”.
(b) handleRainyDay(validMotion, secondsLeft)
• If motion detected:
o Set LED brightness to 255 (FULL).
o Display “RAINY: VEHICLE!” with countdown on
LCD.
• Else:
o Set LED brightness to 150 (MEDIUM).
o Display “CLOUDY/RAINY / MEDIUM LIGHT”.
32
(c) handleEvening(validMotion, secondsLeft)
• If motion detected:
o Set LED brightness to 255 (FULL).
o Display “EVENING: VEHICLE!” with countdown.
• Else:
o Set LED brightness to 100 (DIM).
o Display “DARK EVENING / DIM LIGHT ON”.
(d) handleFullNight(validMotion, secondsLeft)
• If motion detected:
o Set LED brightness to 255 (FULL).
o Display “NIGHT: VEHICLE!” with countdown.
• Else:
o Set LED brightness to 80 (LOW DIM).
o Display “NIGHT: NO VEH / LOW DIM MODE”.
6. Wait for 0.5 Seconds
• Delay 500 ms before the next loop iteration.
7. Repeat Steps 4–6 Continuously
