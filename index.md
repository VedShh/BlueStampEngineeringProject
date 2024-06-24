# Shoulder Recovery Project
This project aids in the recovery process of a rotator cuff injury. This shoulder device uses an Arduino Nano 33 Sense 33 BLE Sense board and alerts the user whenever they make a shoulder movement that is unadvised and puts them backward in their recovery process. The board is being trained using Edge Impulse, where I am recording thousands of data and Edge Impulse is compiling them creating a range of "good movements". 
<!-- Replace this text with a brief description (2-3 sentences) of your project. This description should draw the reader in and make them interested in what you've built. You can include what the biggest challenges, takeaways, and triumphs from completing the project were. As you complete your portfolio, remember your audience is less familiar than you are with all that your project entails! -->

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Vedant S | Monta Vista | Electrical Engineering | Incoming Sophomore

<!-- 
**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**
-->
![Headstone Image](Vedant.png)

<!-- # Final Milestone -->

<!--
**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE

-->

<!-- # Second Milestone -->

<!--
**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/y3VAmNlER5Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your second milestone, explain what you've worked on since your previous milestone. You can highlight:
- Technical details of what you've accomplished and how they contribute to the final goal
- What has been surprising about the project so far
- Previous challenges you faced that you overcame
- What needs to be completed before your final milestone 

-->

# First Milestone

**Summary:**
The end goal of my project is to have a functioning shoulder device that will tell you when you are doing movements that will not worsen your condition of Rotator Cuff Injury. Until now I have set up Edge Impulse, trained my model in Edge Impulse, and then uploaded the code into the Arduino IDE. In Edge Impulse I trained 4 different sets of data: idle, right, left, and up - I did this by simply strapping my Arduino Nano 33 BLE Sense to my arm and doing these movements over and over again with just slight adjustments each time. After I inputted the data, Edge Impulse trained it and gave me a sample code. So far, I have not made edits to the code, but I plan to make the code more customizable later on.

**Challenges:** 
The most frustrating thing of the entire process so far was the lack of instructions and steps that Arduino/Edge Impulse had to be able to set up the project. However, once I overcame the long process of setting up all the tools necessary, things became much easier, but I was not home-free yet. I became excited that I had finally set up everything I immediately jumped into recording data - without fully understanding what I was doing causing:

1) Caused me to record data that was useless (twice!!!),
2) Uploaded code that did not do anything,
3) Mislabing my data which caused massive accuracy issues,
4) Finding the best settings for my data (learning rate, # of epochs, percentage of validation set).
    
When I originally recorded my data I was doing random movements in each class - however, the computer requires very distinct specific movements to be able to learn the most accurately, because of this I went back and manually cleaned/re-record some data that was not distinct enough for the computer. Finding the best settings for the data was another task that took lots of time. First, I had to understand all the settings, after that, I was able to start playing with my data. The thing that made the biggest difference was the learning rate (see Figure 2). Originally the learning rate was too low and after I increased it the accuracy jumped up by 20 percent. This is called micro-controlling and it does make a big difference. 

**What is next?**
The next steps are now to build the circuit and assemble the entire project, with the main step during that next process to CAD and then eventually print out the 3D-printed case which will hold the entire circuit. 

**Figure 2: A graph of what happens when the learning rate is too high/low. The yellow dot is where one starts if the learning rate is too high then the vector will jump around trying to find that optimal point. If the vectors are too small then it make many small changes which will take too much time.**

![Learning Rate](LearningRate.png)

<details>
  <summary>Code</summary>
    NINJA
<details>
```c++
/* Edge Impulse ingestion SDK
 * Copyright (c) 2022 EdgeImpulse Inc.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 * http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 *
 */

/* Includes ---------------------------------------------------------------- */
#include <Bluestamps_Shoulder_Recovery_Project_inferencing.h>
#include <Arduino_BMI270_BMM150.h> //Click here to get the library: https://www.arduino.cc/reference/en/libraries/arduino_bmi270_bmm150/

/* Constant defines -------------------------------------------------------- */
#define CONVERT_G_TO_MS2    9.80665f
#define MAX_ACCEPTED_RANGE  2.0f        // starting 03/2022, models are generated setting range to +-2, but this example use Arudino library which set range to +-4g. If you are using an older model, ignore this value and use 4.0f instead

/*
 ** NOTE: If you run into TFLite arena allocation issue.
 **
 ** This may be due to may dynamic memory fragmentation.
 ** Try defining "-DEI_CLASSIFIER_ALLOCATION_STATIC" in boards.local.txt (create
 ** if it doesn't exist) and copy this file to
 ** `<ARDUINO_CORE_INSTALL_PATH>/arduino/hardware/<mbed_core>/<core_version>/`.
 **
 ** See
 ** (https://support.arduino.cc/hc/en-us/articles/360012076960-Where-are-the-installed-cores-located-)
 ** to find where Arduino installs cores on your machine.
 **
 ** If the problem persists then there's not enough memory for this model and application.
 */

/* Private variables ------------------------------------------------------- */
static bool debug_nn = false; // Set this to true to see e.g. features generated from the raw signal

/**
* @brief      Arduino setup function
*/
void setup()
{
    // put your setup code here, to run once:
    Serial.begin(115200);
    // comment out the below line to cancel the wait for USB connection (needed for native USB)
    while (!Serial);
    Serial.println("Edge Impulse Inferencing Demo");

    if (!IMU.begin()) {
        ei_printf("Failed to initialize IMU!\r\n");
    }
    else {
        IMU.setContinuousMode();
        ei_printf("IMU initialized\r\n");
    }

    if (EI_CLASSIFIER_RAW_SAMPLES_PER_FRAME != 3) {
        ei_printf("ERR: EI_CLASSIFIER_RAW_SAMPLES_PER_FRAME should be equal to 3 (the 3 sensor axes)\n");
        return;
    }
}

/**
 * @brief Return the sign of the number
 * 
 * @param number 
 * @return int 1 if positive (or 0) -1 if negative
 */
float ei_get_sign(float number) {
    return (number >= 0.0) ? 1.0 : -1.0;
}

/**
* @brief      Get data and run inferencing
*
* @param[in]  debug  Get debug info if true
*/
void loop()
{
    ei_printf("\nStarting inferencing in 2 seconds...\n");

    delay(2000);

    ei_printf("Sampling...\n");

    // Allocate a buffer here for the values we'll read from the IMU
    float buffer[EI_CLASSIFIER_DSP_INPUT_FRAME_SIZE] = { 0 };

    for (size_t ix = 0; ix < EI_CLASSIFIER_DSP_INPUT_FRAME_SIZE; ix += 3) {
        // Determine the next tick (and then sleep later)
        uint64_t next_tick = micros() + (EI_CLASSIFIER_INTERVAL_MS * 1000);

        IMU.readAcceleration(buffer[ix], buffer[ix + 1], buffer[ix + 2]);

        for (int i = 0; i < 3; i++) {
            if (fabs(buffer[ix + i]) > MAX_ACCEPTED_RANGE) {
                buffer[ix + i] = ei_get_sign(buffer[ix + i]) * MAX_ACCEPTED_RANGE;
            }
        }

        buffer[ix + 0] *= CONVERT_G_TO_MS2;
        buffer[ix + 1] *= CONVERT_G_TO_MS2;
        buffer[ix + 2] *= CONVERT_G_TO_MS2;

        delayMicroseconds(next_tick - micros());
    }

    // Turn the raw buffer in a signal which we can the classify
    signal_t signal;
    int err = numpy::signal_from_buffer(buffer, EI_CLASSIFIER_DSP_INPUT_FRAME_SIZE, &signal);
    if (err != 0) {
        ei_printf("Failed to create signal from buffer (%d)\n", err);
        return;
    }

    // Run the classifier
    ei_impulse_result_t result = { 0 };

    err = run_classifier(&signal, &result, debug_nn);
    if (err != EI_IMPULSE_OK) {
        ei_printf("ERR: Failed to run classifier (%d)\n", err);
        return;
    }

    // print the predictions
    ei_printf("Predictions ");
    ei_printf("(DSP: %d ms., Classification: %d ms., Anomaly: %d ms.)",
        result.timing.dsp, result.timing.classification, result.timing.anomaly);
    ei_printf(": \n");
    for (size_t ix = 0; ix < EI_CLASSIFIER_LABEL_COUNT; ix++) {
        ei_printf("    %s: %.5f\n", result.classification[ix].label, result.classification[ix].value);
    }
#if EI_CLASSIFIER_HAS_ANOMALY == 1
    ei_printf("    anomaly score: %.3f\n", result.anomaly);
#endif
}

//#if !defined(EI_CLASSIFIER_SENSOR) || EI_CLASSIFIER_SENSOR != EI_CLASSIFIER_SENSOR_ACCELEROMETER
//#error "Invalid model for current sensor"
//#endif

```

<!--

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/CaCazFBhYKs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your first milestone, describe what your project is and how you plan to build it. You can include:
- An explanation about the different components of your project and how they will all integrate together
- Technical progress you've made so far
- Challenges you're facing and solving in your future milestones
- What your plan is to complete your project

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
-->

# Bill of Materials

<!--

Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

-->
| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Arduino Nano33 BLE Sense | Microcomputer used to store code identifying different movements that user makes | $34.80 | <a href="https://store-usa.arduino.cc/products/nano-33-ble-sense-rev2-with-headers?gad_source=1&gclid=CjwKCAjwg8qzBhAoEiwAWagLrGZpO0uCQlcYXwoyQo1uV1hVKdxjny7cgm1z-Cc4NqsN9SuL9b_EGxoCO8AQAvD_BwE"> Link </a> |
| OLED Screen | To display user's progress with arm movements | $5.00 | <a href="https://geekworm.com/products/0-96-inch-oled?variant=39982251671640&currency=USD&utm_medium=product_sync&utm_source=google&utm_content=sag_organic&utm_campaign=sag_organic&srsltid=AfmBOoo_Xjfk_CSJ04SWm9pKG7w3IMF9L5JKRlc9OFoA2WHD6CZBDicif4k&com_cvv=8fb3d522dc163aeadb66e08cd7450cbbdddc64c6cf2e8891f6d48747c6d56d2c"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |



# Starter Project

<iframe width="560" height="315" src="https://www.youtube.com/embed/qdelkj9V17M?si=obpftZMP8jghWpjv" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

**Summary:**
I built a small circuit on the Arduino Uno for my starter project. The circuit gets input from the motion sensor (refer to Figure 1) which either gives us a 'high' or 'low' data value. If the motion sensor does not detect motion then a red LED is lit up and if it does detect motion then a green LED is lit up along with a pienzo buzzer making a buzzing sound providing an audio distinction. I coded this with a simple if-else checking that if the motion sensor returns 'high' then we light up the green LED + piezo buzzer, else we turn on the red LED. I also added a potentiometer to control the pitch of the piezo buzzer's sound. We have to put the potentiometer on the analog side because we incrementally change the pitch. Some challenges that I faced when doing this starter project were mainly my lack of knowledge of the tools and the components. After I learned the basics of Arduino, how to wire, how different sensors work, and the main wiring conventions, I was able to build circuits much faster.

**Figure 1: A basic motion sensor with 3 ports where power(VCC), ground(GND), and a digital port(OUT) are connected**

![Eduction Image](motionSensor.png)



<!--# Other Resources/Examples -->

<!--

One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.

-->
