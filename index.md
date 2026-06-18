# BlueStamp Project
Replace this text with a brief description (2-3 sentences) of your project. This description should draw the reader in and make them interested in what you've built. You can include what the biggest challenges, takeaways, and triumphs from completing the project were. As you complete your portfolio, remember your audience is less familiar than you are with all that your project entails!

You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:
```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
```

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Sai C | San Juan Hills High School | Biomedical Engineering | Incoming Senior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# First and Second Milestone

https://youtu.be/irftEeXKm-8

My project is an armband that is aimed to recognize when someone who is supposed to have limited mobility (due to surgery, broken bones, etc.) moves their arm passed a certain threshold as well as when they are in unsafe temperature conditions.  The components of my project include an Arduino Nano ESP32, a DS18B20 Temperature Sensor, an Accelerometer, a Piezo Speaker, and a Vibration buzzer. Through both milestones, I was able to not only test the components individually (including a Force Resistive Sensor that I did not end up using at this stage), but I also connected each one and made the base of the device, in which both the temperature sensor and accelerometer set off the speaker and buzzer to alert the armband wearer if they are in too hot of a condition, are moving their arm too mucch, or both. Some challenges I faced when making this project include the accelerometer not registering with the Arduino, and through many tests I was eventually able to figure out with the help of my instructors that the issue was setemming from the fact the A4 and A5 pins on my ESP32 were not working, and this was important because they are the only pins that work with the accleromter. To fix this, we ordered a new ESP32 with working pins. The next issue I struggled with was the temperature sesnor, as the DS18B20 was not working with the Nano, so for the time being I replaced the temperature sensor with a photoresister which measures ambient lighting for proof of concept, and in my video I demonstrate how I used the measurements taken to create a "Fake Temperature" which transfers the data found from the lighting into a readable temperature. My second miletsone was completeing the board and combining all the components on my breadboard, and I displayed this in my video. Each of the sensors trigger a certain pattern in the buzzer and speaker based on if they pass the "dangerous" threshold for the armband wearer, and there is a pattern for if both are occuring. Something that has been suprising about the project so far is that it has really taught me how to troubleshoot errors, because my project so hevaily relies on multiple different components, and if one doesn't work it sets off the rest of the project, so I did not expect for that to be such a large learning curve for me. To complete my final milestone, I am hoping to figure out my temperature sesnor and add it back in, as well as attatch my board to the armband apparatus, and figure out the logistics of that. I also hope to see if there is anyway for me to re-include the FSR in my final version.



# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
#include <Wire.h> // Used for I2C communication with the MPU6050 accelerometer
 // Output pins
const int motorPin = D9; // Vibration motor signal pin
const int buzzerPin = D8; // Passive piezo buzzer pin
const int buttonPin = D2; // Button pin
// Input pin
const int lightPin = A1; // Photoresistor pin, pretending to be temperature
// MPU6050 settings
const int MPU_ADDR = 0x68; // Default I2C address for MPU6050
bool accelFound = false; // Tracks whether accelerometer was detected
int16_t baseX, baseY, baseZ; // Starting/normal accelerometer position
// Alert states
bool motionAlert = false; // True when motion alert is active
bool tempAlert = false; // True when fake temperature alert is active
bool alertsMuted = false; // True after button is pressed to silence alert
// Motion threshold in g units
// Same as old raw threshold of 12000: 12000 / 16384 = about 0.73g
const float motionThresholdG = 0.73;
// Fake temperature limits
const float lowTempC = 35.0; // Low fake body temp limit
const float highTempC = 38.0; // High fake body temp limit
void setup() {
    Serial.begin(115200); // Start Serial Monitor
    delay(2000); // Give Serial Monitor time to open
    pinMode(motorPin, OUTPUT); // Motor is an output
    pinMode(buzzerPin, OUTPUT); // Buzzer is an output
    pinMode(buttonPin, INPUT_PULLUP); // Button uses internal pull-up resistor
    digitalWrite(motorPin, LOW); // Motor off at start
    noTone(buzzerPin); // Buzzer off at start
    Serial.println("Patient alert project starting...");
    // Start accelerometer communication
    Wire.begin();
    // Check if MPU6050 is connected
    Wire.beginTransmission(MPU_ADDR);
    byte error = Wire.endTransmission();
    if (error == 0) {
        Serial.println("MPU6050 found!");
        accelFound = true;
        // Wake up MPU6050
        Wire.beginTransmission(MPU_ADDR);
        Wire.write(0x6B); // Power management register
        Wire.write(0); // Wake sensor up
        Wire.endTransmission();
        delay(500);
        // Save current position as the normal starting position
        readAccelerometer(baseX, baseY, baseZ);
    } else {
        Serial.println("MPU6050 not found. Motion alert disabled.");
        accelFound = false;
    }
}
void loop() {
    // If button is pressed, mute current alerts
    if (buttonPressed()) {
        clearAlerts();
        alertsMuted = true;
        Serial.println("Alerts muted by button.");
        delay(500); // Simple debounce delay
        return;
    }
    checkMotion(); // Check accelerometer motion
    checkFakeTemperature(); // Check fake temp from photoresistor
    // If both alerts are no longer active, allow future alerts again
    if (!motionAlert && !tempAlert) {
        alertsMuted = false;
    }
    // If alerts were muted, keep motor and buzzer off
    if (alertsMuted) {
        stopOutputs();
        delay(100);
        return;
    }
    // Pick the correct alert pattern
    if (motionAlert && tempAlert) {
        playBothAlert(); // Continuous alert
    } else if (motionAlert) {
        playMotionAlert(); // Long beep pattern
    } else if (tempAlert) {
        playTempAlert(); // Double beep pattern
    } else {
        stopOutputs(); // Nothing wrong, stay quiet
    }
    delay(100);
}
void checkMotion() {
    if (!accelFound) return; // Skip if accelerometer was not detected
    int16_t x, y, z; // Current accelerometer readings
    readAccelerometer(x, y, z);
    // Compare current position to starting position
    int rawMovement = abs(x - baseX) + abs(y - baseY) + abs(z - baseZ);
    // Convert raw movement to approximate g units
    float movementG = rawMovement / 16384.0;
    Serial.print("Movement G: ");
    Serial.println(movementG);
    // If movement is big enough, turn on motion alert
    if (movementG > motionThresholdG) {
        motionAlert = true;
    }
}
void checkFakeTemperature() {
    int lightValue = analogRead(lightPin); // Read photoresistor value
    // Convert light reading to fake body temp from 35C to 42C
    float fakeTempC = 35.0 + (lightValue / 4095.0) * 7.0;
    Serial.print("Light: ");
    Serial.print(lightValue);
    Serial.print(" | Fake Temp C: ");
    Serial.println(fakeTempC);
    // If fake temp is outside the safe range, trigger temp alert
    if (fakeTempC < lowTempC || fakeTempC > highTempC) {
        tempAlert = true;
    }
}
void playMotionAlert() {
    // Pattern: beeeep ... beeeep ... beeeep
    digitalWrite(motorPin, HIGH); // Motor on
    tone(buzzerPin, 1000); // Buzzer on
    delay(600); // Long beep
    digitalWrite(motorPin, LOW); // Motor off
    noTone(buzzerPin); // Buzzer off
    delay(400); // Pause
}
void playTempAlert() {
    // Pattern: beepbeep ... beepbeep ... beepbeep
    for (int i = 0; i < 2; i++) {
        digitalWrite(motorPin, HIGH); // Motor on
        tone(buzzerPin, 1500); // Buzzer on
        delay(150); // Short beep
        digitalWrite(motorPin, LOW); // Motor off
        noTone(buzzerPin); // Buzzer off
        delay(150); // Short pause
    }
    delay(500); // Longer pause between double-beep groups
}
void playBothAlert() {
    // Pattern: continuous beeeeeeeeeep
    digitalWrite(motorPin, HIGH); // Motor stays on
    tone(buzzerPin, 2000); // Buzzer stays on
}
void clearAlerts() {
    motionAlert = false; // Clear motion alert
    tempAlert = false; // Clear temp alert
    stopOutputs(); // Turn off motor and buzzer
}
void stopOutputs() {
    digitalWrite(motorPin, LOW); // Motor off
    noTone(buzzerPin); // Buzzer off
}
bool buttonPressed() {
    // Button uses INPUT_PULLUP, so pressed = LOW
    return digitalRead(buttonPin) == LOW;
}
void readAccelerometer(int16_t & x, int16_t & y, int16_t & z) {
    // Ask MPU6050 for accelerometer data
    Wire.beginTransmission(MPU_ADDR);
    Wire.write(0x3B); // First accelerometer register
    Wire.endTransmission(false);
    // Request 6 bytes: X high/low, Y high/low, Z high/low
    Wire.requestFrom(MPU_ADDR, 6, true);
    // Combine two bytes for each axis
    x = Wire.read() << 8 | Wire.read();
    y = Wire.read() << 8 | Wire.read();
    z = Wire.read() << 8 | Wire.read();
}

```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
