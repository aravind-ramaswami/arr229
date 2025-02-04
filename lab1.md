# LAB 1: Artemis and Bluetooth
The goal of this lab was to familiarize us with the microcontroller (Artemis Nano) that we would be using for the rest of the semester. In this lab, I tested out the basic functionality of the microcontroller and established a Bluetooth connection between the Artemis Nano and my computer. This lab has two parts, which are described below.

# Part A:
Prelab: I downloaded the Arduino IDE and installed the necessary libraries. 

Task 1: Blink

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZFbKsTz90jE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


Task 2: Serial

[![Serial](https://youtu.be/W-cWZUY_tTY/0.jpg)](https://youtu.be/W-cWZUY_tTY)

I had to lower the baud rate to 9600 from 115200 to prevent my computer from crashing.

Task 3: Temperature Sensor

[![Temperature Sensor](https://youtu.be/ZFbKsTz90jE/0.jpg)](https://youtu.be/ZFbKsTz90jE)

You can see how the temperature data changes when I blow on it, confirming that it works. 

Task 4: Microphone Output

[![Microphone](https://youtu.be/Vjr9ALpmdrc/0.jpg)](https://youtu.be/Vjr9ALpmdrc)

I tested the Microphone to confirm its functionality and gain more experience with the programming language. The microphone picks up the finger and snaps. 

5000 Level Tasks

Task 1: Blink on "C" note

![Blink C Code](https://github.com/user-attachments/assets/15cfe333-2fb6-40f2-a185-c1eac448ab0c)

[![Blink C Video](https://youtube.com/shorts/VY-fb7THgdQ?feature=share/0.jpg)](https://youtube.com/shorts/VY-fb7THgdQ?feature=share)

In this task, I wrote code that caused the Artemis to turn on its LED whenever a musical "C" note was detected. 


# Part B:

Prelab: This section of the lab established a Bluetooth connection between the Artemis and my computer.  For the prelab, I ran the commands shown in the figure below. 

![image](https://github.com/user-attachments/assets/c4176822-2773-4629-8c25-4700cc630acb)

After setting up the virtual environment, I downloaded the given Ble codebase, burned it on the Artemis, and executed it to confirm that the Artemis displayed a Mac Address. 

![image](https://github.com/user-attachments/assets/f6be6483-9110-43e0-b813-3d05f5002965)

I also generated a new uuid that enables the computer to identify the Artemis and send data to it. 

![image](https://github.com/user-attachments/assets/d814f4ac-91f3-48c4-aba6-1138a8d2ace4)   

![image](https://github.com/user-attachments/assets/8658af19-5ec1-4a83-93df-4dc0567ebd88)

The provided codebase establishes the basic functionality for Bluetooth connection between the Artemis and a computer.  When the computer sends a message, this file uses the uuids to receive the message, process it, and send back a reply (if desired). BLECStringCharacteristic.h EString.h and RobotCommand.h are helper files that define different types of message data that you can send to the computer. Finally, ble_arduino.ino handles the actual Bluetooth communication. 

Task 1: ECHO

I programmed the Artemis to send back a modified message when it received a reply. If the message sent was "x", the returned message was "Robot says ->x:)". 

![image](https://github.com/user-attachments/assets/5f3b7a6d-b397-4f46-a317-25d6c1a62faf)

![image](https://github.com/user-attachments/assets/c25ed099-22ae-4d5d-b190-eb56540835e0)

Task 2: SEND_THREE_FLOATS

I programmed the Artemis to decompose a message sent by the computer and output those floats to the serial screen. 

![image](https://github.com/user-attachments/assets/dc3727b2-8008-4eae-91e7-4f76cab790d5)

Python code:

![image](https://github.com/user-attachments/assets/50630503-d791-487d-bb83-79f5654ad9be)

![image](https://github.com/user-attachments/assets/2a5526f2-069a-4b04-b4da-241127812b0d)

Task 3: GET_TIME_MILLIS

I used the built-in millis() command on the Artemis to complete this task. 

![image](https://github.com/user-attachments/assets/d9b3ceba-8f6d-4ade-9948-c4d4424cb629)

![image](https://github.com/user-attachments/assets/04291a20-a5ab-47b5-839e-611dc8b1ed3f)

Task 4: Notification Handler

I set up a notification handler in Python that will receive a message and extract the time.  

![image](https://github.com/user-attachments/assets/8af4f393-1c05-4a03-a264-1d532bab3529)

Task 5: DATA RATE

To test the data rate, I programmed a loop on the Artemis which sent the current time for 6 seconds. 

![image](https://github.com/user-attachments/assets/4f558205-f1ce-424d-b76e-ba0f939d0a0b)

I used the notification handler to receive these messages and print them on the Jupyter Notebook console. 

![image](https://github.com/user-attachments/assets/fa045aca-80eb-4dce-b1cd-dddb0b1832a9)

I collected 242 messages in 6 seconds, so the Artemis can send about 40.3 messages/second. 

Task 6: SEND_TIME_DATA

I programmed the Artemis to collect 20 timestamps in an array, and send those timestamps to the computer. 

![image](https://github.com/user-attachments/assets/28ff837a-efba-4cdf-8013-4d3b412665ea)

I wrote this notification handler (joint for Tasks 6 and 7) to parse the incoming data and append it to a list. 

![image](https://github.com/user-attachments/assets/e0affb00-89cc-43ee-9215-fe093bd5cff5)

I tested out this message with the following Python code. 

![image](https://github.com/user-attachments/assets/8548af49-f13a-4acb-98d5-6c5dbbc6c19f)

Task 7: GET_TEMP_READINGS

I created another command in the Artemis that would append the current time and temperature for 20 iterations using the millis() and getTempDegF() commands into two separate lists.

![image](https://github.com/user-attachments/assets/8194a7fc-105a-4afc-9d50-5b0b62df5f78)

I used the same notification handler shown in task 6. It splits the incoming data into time and temperature data before appending them to separate lists. 

![image](https://github.com/user-attachments/assets/e0affb00-89cc-43ee-9215-fe093bd5cff5)

![image](https://github.com/user-attachments/assets/bdc4d78f-ebe7-4f41-9a15-a6404b705551)

Task 8: Method comparison

The 1st method sends data in real-time as it is received while the 2nd method collects data over an interval before very rapidly sending that data. The 2nd method can send data very rapidly. Investigating the output from the 2nd method, we can see that several timestamps have the same value, implying that Artemis sent multiple data points within 1 ms. 

The advantage of the 1st method is that it allows for real-time data communication, but is slower since you have to wait for new sensor data. The 2nd method has a very fast data transfer rate but is not real-time data. This could be a problem if the robot moves very fast, as the robot will move to a new location by the time it sends data about the previous location. Additionally, this method will store data on the Artemis, so it could run out of storage. The 1st method should be used if you need real-time updates about the robot or want to debug the robot. The 2nd method should be used if you want to record data from a stunt or sensor for later processing and analysis. 

The Artemis has 384kb or 384000 bytes of storage. Since the time and temperature values were stored as floats, each message's size was 4 bytes. Thus, at a maximum, the Artemis can store 96000 time values. 

5000 Level Tasks:

Task 1: Effective Data Rate and Overhead

The Artemis code processed an incoming message, created a message of a certain byte size, and sent it to the array. 

![image](https://github.com/user-attachments/assets/de9460ed-bd55-4751-9311-89eb01a96b06)

The corresponding Python code sends queries for different-sized messages and computes the data rate from the send and return time.

![image](https://github.com/user-attachments/assets/0fd5a65b-c06b-4674-87b8-9dc7876b5e67)

![image](https://github.com/user-attachments/assets/43e4c40c-ec55-47bd-8821-f5e013f78048)

From this, we can see that sending short packets of data introduces a significant amount of overhead, limiting the data rate. As an example, the data rate for 5-byte messages was 29.14 bytes/s and the data rate for 120-byte messages was 499.3 bytes/s. 

Task 2: Reliability

I programmed the Artemis to collect 20 timestamps in a list and then send that array to the computer. To test reliability, I sent messages with different delay times ([0,10,50,100,200]), comparing the number of sent messages with the number received. 

![image](https://github.com/user-attachments/assets/901e8dab-4518-4642-a014-a3d21f701fbc)

On the computer side, I wrote a notification handler that added the incoming data to a list and printed both the list and the length of the list. 

![image](https://github.com/user-attachments/assets/a1adb08c-e856-40df-9af8-c6da831f0861)

At each delay time, the computer received all the messages that the Artemis sent. From this, I can conclude that the BLE connection is reliable. 

# Discussion

In this lab, I learned how to set up and send Bluetooth information between the Artemis and the computer. I also brushed up on C and Python coding skills, which will help me in future labs. 







