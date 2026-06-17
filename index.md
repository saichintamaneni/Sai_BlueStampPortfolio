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

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/y3VAmNlER5Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

My project is an armband that is aimed to recognize when someone who is supposed to have limited mobility (due to surgery, broken bones, etc.) moves their arm passed a certain threshold as well as when they are in unsafe temperature conditions.  The components of my project include an Arduino Nano ESP32, a DS18B20 Temperature Sensor, an Accelerometer, a Piezo Speaker, and a Vibration buzzer. Through both milestones, I was able to not only test the components individually (including a Force Resistive Sensor that I did not end up using at this stage), but I also connected each one and made the base of the device, in which both the temperature sensor and accelerometer set off the speaker and buzzer to alert the armband wearer if they are in too hot of a condition, are moving their arm too mucch, or both. Some challenges I faced when making this project include the accelerometer not registering with the Arduino, and through many tests I was eventually able to figure out with the help of my instructors that the issue was setemming from the fact the A4 and A5 pins on my ESP32 were not working, and this was important because they are the only pins that work with the accleromter. To fix this, we ordered a new ESP32 with working pins. The next issue I struggled with was the temperature sesnor, as the DS18B20 was not working with the Nano, so for the time being I replaced the temperature sensor with a photoresister which measures ambient lighting for proof of concept, and in my video I demonstrate how I used the measurements taken to create a "Fake Temperature" which transfers the data found from the lighting into a readable temperature. My second miletsone was completeing the board and combining all the components on my breadboard, and I displayed this in my video. Each of the sensors trigger a certain pattern in the buzzer and speaker based on if they pass the "dangerous" threshold for the armband wearer, and there is a pattern for if both are occuring. Something that has been suprising about the project so far is that it has really taught me how to troubleshoot errors, because my project so hevaily relies on multiple different components, and if one doesn't work it sets off the rest of the project, so I did not expect for that to be such a large learning curve for me. To complete my final milestone, I am hoping to figure out my temperature sesnor and add it back in, as well as attatch my board to the armband apparatus, and figure out the logistics of that. I also hope to see if there is anyway for me to re-include the FSR in my final version.



# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  Serial.println("Hello World!");
}

void loop() {
  // put your main code here, to run repeatedly:

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
