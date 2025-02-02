# LAB 1: Artemis and Bluetooth
The goal of this lab was to familiarize us with the microcontroller (Artemis Nano) that we would be using for the rest of the semester. In this lab, I tested out the basic functionality of the microcontroller and established a Bluetooth connection between the Artemis Nano and my computer. This lab has two parts, which are described below.

# Part A:
Prelab: I downloaded the Arduino IDE and installed the necessary libraries to program the Artemis Nano using the Arduino IDE. 

Task 1: Blink

[![Blink](https://youtube.com/shorts/03luXKfBtho?feature=share/0.jpg)](https://youtube.com/shorts/03luXKfBtho?feature=share)

In this task, I loaded a default example program into the Artemis Nano that blinked its LED. This tested the basic functionality of the board. 

Task 2: Serial

[![Serial](https://youtu.be/W-cWZUY_tTY/0.jpg)](https://youtu.be/W-cWZUY_tTY)

I tested the serial communication between the Artemis and the computer. I had to lower the baud rate to 9600 from 115200 to prevent my computer from crashing.

Task 3: Temperature Sensor

[![Temperature Sensor](https://youtu.be/ZFbKsTz90jE/0.jpg)](https://youtu.be/ZFbKsTz90jE)
I tested the onboard temperature sensor on this board. You can see how the temperature data changes when I blow on it, confirming that it works. 

Task 4: Microphone Output

[![Microphone](https://youtu.be/Vjr9ALpmdrc/0.jpg)](https://youtu.be/Vjr9ALpmdrc)

I tested the onboard Microphone to confirm its functionality and gain more experience with the programming language. In this video, you can see how the frequency count spikes whenever the snap occurs. 

5000 Level Tasks

Task 1: Blink on "C" note

![Blink C Code](https://github.com/user-attachments/assets/15cfe333-2fb6-40f2-a185-c1eac448ab0c)

[![Blink C Video](https://youtube.com/shorts/VY-fb7THgdQ?feature=share/0.jpg)](https://youtube.com/shorts/VY-fb7THgdQ?feature=share)

In this task, I wrote code that caused the Atermis to turn on its LED and leave it on whenever a musical "C" note was played. Otherwise, the LED was off. The image and video above show the associated code and a demonstration. 



# Part B:

Prelab: This section of the lab established bluetooth connection between the Artemis and my computer. It will be useful for sending data in the future. For the prelab, I setup a virtual environment called "FastRobots_ble" for this lab. In this environment, I installed these packages, "numpy pyyaml colorama nest_asyncio bleak jupyterlab", to gain the neccessary functionality. Finally, I downloaded the provided codebase, and got the arduino to print the mac adress. 

--insert image showing this--

--insert image showing configured ids on the jupyter notebook--

The provided codebase establishes the basic functionality for bluetooth connection between the Artemis and a computer. 

Task 1: ECHO

Task 2: SEND_THREE_FLOATS

Task 3: GET_TIME_MILLIS

Task 4: Notification Handler

Task 5: DATA RATE

Task 6: SEND_TIME_DATA

Task 7: GET_TEMP_READINGS

Task 8: Method comparison

5000 Level Tasks:

Task 1:

Task 2:

Task 2:




