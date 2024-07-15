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

# Final Milestone


**Summary:**
My final milestone for this project is to upload my model onto the Arduino Nano 33 Sense BLE and print out the reading that the Arduino is getting onto the OLED screen. I had to redo my model because when I first made the model, I only checked the training accuracy, without looking at the testing accuracy. When I eventually looked at the testing accuracy, I realized that the model was memorizing the training data which is why the training accuracy was so high and the testing accuracy was so low.  After this, I tried running my model in my terminal and realized that some of my movements and classes overlap, making it very hard for the model to distinguish the different shoulder movements into distinct classes. 

After this, I thought that the best way to continue was to restart my model. I chose 4 new classes based on exercises: full bow, arm circle, lateral, and idle(regular movement). I collected more data and got a fully trained model with 100% training accuracy and 97% test accuracy. After this, for some reason, when I tried deploying my model onto the Arduino IDE the predictions that it would make would be random predictions. This took a lot of time to troubleshoot and fix.

![TrainingRate](FinalTraining.png)

**Figure 9: This is a picture of my finished training model and the training accuracy.**

Once I could deploy my model, I coded the OLED as a progress bar. The user has to make certain movements to help them in their process of shoulder rehabilitation. As they do the movements (ex: 10 lateral raises, 10 arm circles, etc) a progress bar fills up as soon as they have done the exercise. 


**Challenges:**
I have faced many challenges during this final milestone. The first major one is when I realized that my model was not doing well in the testing data. As explained above, I had to restart my model and decided to classify the data more distinctly. I separated the shoulder movements that had little to no overlap. I decided to do exercises because different exercises specifically target different parts of the shoulder so it would be the most efficient way to create the classes. When the classes and data overlap, your model can become inaccurate. 

The second major challenge that I faced was with deploying my code into Arduino. For some reason whenever I deployed my code from Edge Impulse onto the Arduino IDE it would misclassify the data as sometimes just guess. I tried to import many different versions of the code to see if one would work once I deployed it onto the Arduino. Eventually, my instructor and I figured out that my target device was not set to the Nano. Once I changed that, I deployed it onto the Arduino IDE and it worked!

**What's next?**
Now that I have finished my project I can start working on my modifications. Some ideas that I have so far are: 
- Connecting AI to my Arduino giving live suggestions on the spot and then printing them out onto the OLED
- Connecting the Arduino to my phone and making a small app where I can display data and suggestions to

<!--
**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE

-->

# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/pXe6PBbaICc?si=IjKu5BtEdx9hOn4C" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

**Summary:**
The second milestone for my project is to CAD a 3D-printed box on Fusion 360 (see Figure 7), build my circuit, and fit the circuit inside of the box. (cont.)

<img src="ShoulderCAD.png" alt="Fusion CAD" width="500" height="400"> <img src="BSEbox.png" alt="FinalBox" width="450" height="400">

<!--
replace ![Fusion CAD](ShoulderCAD.png) with <width="250">

![Fusion CAD](ShoulderCAD.png){:height="250px" width="250px"}            ![FinalBox](BSEbox.png){:height="250px" width="250px"}

![Fusion CAD](ShoulderCAD.png)
-->
**Figure 7: This is a screenshot of my final CAD on Fusion 360.**         **Figure 8: This is the second version of my fully CADed box.**
**I have 4 separate bodies (the main body, the OLED cover, the**          **I have uploaded some scroll code and all the circuits work inside.**
**switch cover, and the slider lid).**

My circuit (see Figure 6) includes 3 parts: Arduino Nano BLE Sense (microcontroller), OLED screen, and the on/off switch. The main circuit is between the Arduino and the OLED screen, the switch rests between the power cable that goes from the Arduino to the OLED screen. When the switch turns on it connects the power cables, but when it is off it disables the connection. The other parts of the circuit include ground, SCL, and SDA wires. 

![Circuit](ShoulderCircuit.png)

**Figure 6: This is a photo of my circuit.**

These wires are part of the I^2C protocol (see Figure 5) which first starts with identifying which port the target device is connected to. Once it identifies this then it starts the clock which switches from high to low in intervals so that the data is transferred efficiently. The data is only transfered on the rising edge which is how the data is controlled. The SCL line that is used to synchronously clock data in and out of a device. The SDA line is used to transmit data. 

![I2C Circuit](I2C.png)

**Figure 5: This is a photo of the I2C protocol. It first starts with the SDA line switching from high to low voltage, then the SCL line switches from high to low. Then the address frame is a 7 or 10-bit sequence that is unique to each "slave" (target device) and identifies the slave when the master (Arduino) wants to talk. The next message lets the slave know whether or not the master is going to be sending data. Every message is followed by an ACK/NACK bit which is returned to the sender if the data was successfully received.**

**Challenges:** 
Some challenges that I have faced during this time have been trying to optimize space and function. The process of CADing could have been a lot quicker if I had disregarded space, however, I wanted this project to have some practical uses, and having a massive block on my shoulder would be uncomfortable. Because of this, I tried to minimize the space used by the parts to have the smallest block possible. 

At first, I was trying to find different switches that would take up less space however, after lots of time, I realized that the switch that I wanted to implement was running an AC current which was not compatible with my circuit which ran on a DC current. After building my circuit I started CADing and came up with a design. However, after printing it out I realized that I had not calculated for tolerance, which is making all the dimensions slightly larger to calculate for the error that the 3D printer makes. Since originally I had not calculated for tolerance none of my components fit into my box. When the box printed out I also realized that I could make my box even smaller by layering the components inside. 


**What is next?**
Now I will make some small refinements to my model, upload the model to the Arduino Nano BLE, and then start on my modifications. 


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

<iframe width="560" height="315" src="https://www.youtube.com/embed/kX5n4q9cTTE?si=_l9Z2tKIlmfRK0sR" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


**Summary:**
The first milestone of my project is to deploy a machine-learning model onto an Arduino Nano BLE Sense 33 with Edge Impulse. I ran this with the terminal and it printed out how confident it was with predicting a certain movement that I had made (see below at Figure 4).

![PredictionsExamples](Predictions1.png)

**Figure 4: This is an example screenshot of the predictions that my model made. You can see that it makes a prediction of x% on one of the 4 classes that I have made.**

The first step would be to connect a device. Then the machine workflow starts with data acquisition, then you have to preprocess the data, then design the neural network (see Figure 3 for my example), and finally, we can train our model. Currently, I uploaded the code that Edge Impulse made directly onto the Nano Sense 33, which requires a wired connection with the computer. Eventually, I want the code to be deployed wirelessly on the Nano itself.

Until now I have set up Edge Impulse on my computer using the CLI (command line interface), trained my model in Edge Impulse, and then uploaded the code onto the nano.

In Edge Impulse I trained 4 different sets of data: idle, right, left, and up. I strapped my Arduino Nano 33 BLE Sense to my arm and did these movements repeatedly with just slight adjustments each time. Slight adjustments led to various data so the model could become generalized. After I inputted the data, I kept customizing certain settings in each step of the workflow (more on this further down), eventually getting a pretty accurate model. So far, I have not made edits to the code, but I plan to make the code more customizable later on.

**Challenges:** 
The most frustrating thing of the entire process so far was the lack of instructions and steps that Arduino/Edge Impulse had to be able to set up the project. However, once I overcame the long process of setting up all the tools necessary, things became much easier, but I was not home-free yet. I became excited that I had finally set up everything I immediately jumped into recording data - without fully understanding what I was doing causing:

1) Caused me to record data that was useless (twice!!!),
2) Uploaded code that did not do anything,
3) Mislabeling my data which caused massive accuracy issues,
4) Finding the hyperparameters for my data (learning rate, # of epochs, percentage of validation set).

When I originally recorded my data I was doing random movements in each class - however, the computer requires very distinct specific movements to be able to learn the most accurately, because of this I went back and manually cleaned/re-record some data that was not distinct enough for the computer. Finding the best hyperparameters for the data was another task that took lots of time. First, I had to understand all the settings, after that, I was able to start playing with my data. The thing that made the biggest difference was the learning rate (see Figure 2). Originally the learning rate was too low and after I increased it the accuracy jumped up by 60 percent.

**What is next?**
The next steps are now to build the circuit and assemble the entire project, with the main step during that next process to CAD and then eventually printing out the 3D-printed case which will hold the entire circuit. Another big step would be to deploy my code onto the Arduino IDE while also increasing some accuracy. 

<!--
<details>
  <summary><b>Code for Milestone 1: Edge Impulse Model</b></summary>
</details>
-->

![Nueral Network](NeuralNetwork.png)

**Figure 3: This is a screenshot of the neural network that I created for my data set. I wanted a deep and wide neural network as I had lots of data and there needed to be lots of processing power. To create a wide network I increased the number of neurons in each layer to 48, and to create a deep network I added 3 separate layers of 48 neurons. I also added a drop rate after each layer of 0.2. This forces the machine to learn as if there is not a drop rate then the machine will just memorize the path of your training data giving you a false accuracy. This drops out a few neurons each layer making the machine learn different patterns.**


![Learning Rate](LearningRate.png)

**Figure 2: A graph of what happens when the learning rate is too high/low. The yellow dot is where one starts if the learning rate is too high then the vector will jump around trying to find that optimal point. If the vectors are too small then it makes many small changes which will take too much time. (See Learning Rate link at the bottom of the page)**

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


![Eduction Image](motionSensor.png)

**Figure 1: A basic motion sensor with 3 ports where power(VCC), ground(GND), and a digital port(OUT) are connected. (See Motion Sensor link at the bottom of the page)**


# Works Cited

- [Learning Rate](https://www.jeremyjordan.me/nn-learning-rate/)
- [Motion Sensor](https://lastminuteengineers.com/pir-sensor-arduino-tutorial/)
- [I2C Protocol](https://www.circuitbasics.com/basics-of-the-i2c-communication-protocol/)
  
<!--

One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.

-->
