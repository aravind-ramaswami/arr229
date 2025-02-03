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

Prelab: This section of the lab established a Bluetooth connection between the Artemis and my computer. It will be useful for sending data in the future. For the prelab, I ran the commands shown in the figure below. These commands set up my virtual environment and installed the required packages. 

![image](https://github.com/user-attachments/assets/c4176822-2773-4629-8c25-4700cc630acb)

After setting up the virtual environment, I downloaded the given Ble codebase, burned it on the Artemis, and executed it to confirm that the Artemis displayed a Mac Address. The Artemis successfully displayed the following Mac address. 

![image](https://github.com/user-attachments/assets/f6be6483-9110-43e0-b813-3d05f5002965)

After setting up the Mac address, I modified the connections.yaml file to include this Mac address. I also generated a new uuid that enables the computer to identify the Artemis and send data to it. The following images show how the Mac address and uuid strings are the same for the Artemis and the computer. 

![image](https://github.com/user-attachments/assets/d814f4ac-91f3-48c4-aba6-1138a8d2ace4)   

![image](https://github.com/user-attachments/assets/8658af19-5ec1-4a83-93df-4dc0567ebd88)


The provided codebase establishes the basic functionality for Bluetooth connection between the Artemis and a computer.  The uuids are unique identifiers that enable the computer to communicate with the Artemis and denote different types of data that you want to send. BLECStringCharacteristic.h EString.h and RobotCommand.h are helper files that define different types of message data that you can send to the computer. Finally, ble_arduino.ino handles the actual Bluetooth communication. When the computer sends a message, this file uses the uuids to receive the message, process it, and send back a reply (if desired). 

Task 1: ECHO

I programmed the Artemis to send back a modified message when it received a reply. If the message sent was "x", the returned message was "Robot says ->x:)". The code and successful demonstration are shown below. 

![image](https://github.com/user-attachments/assets/5f3b7a6d-b397-4f46-a317-25d6c1a62faf)

![image](https://github.com/user-attachments/assets/c25ed099-22ae-4d5d-b190-eb56540835e0)

Task 2: SEND_THREE_FLOATS

I programmed the Artemis to decompose a message sent by the computer consisting of three floats and output those floats to the serial screen. This is code on the Artemis board.

![image](https://github.com/user-attachments/assets/dc3727b2-8008-4eae-91e7-4f76cab790d5)

The following images show a successful test of this command. 

![image](https://github.com/user-attachments/assets/50630503-d791-487d-bb83-79f5654ad9be)

![image](https://github.com/user-attachments/assets/2a5526f2-069a-4b04-b4da-241127812b0d)

Task 3: GET_TIME_MILLIS

I used the built-in millis() command on the Artemis to establish this. The code on the Artemis and computer sides is shown below. The computer side also shows a test of this command, where the current time is queried from the Artemis. 

![image](https://github.com/user-attachments/assets/d9b3ceba-8f6d-4ade-9948-c4d4424cb629)

![image](https://github.com/user-attachments/assets/04291a20-a5ab-47b5-839e-611dc8b1ed3f)

Task 4: Notification Handler

I set up a notification handler in Python that will receive a message and extract the time.  

![image](https://github.com/user-attachments/assets/8af4f393-1c05-4a03-a264-1d532bab3529)

Task 5: DATA RATE

I wanted to test the data transfer rate between the Artemis and the computer. I programmed a loop on the Artemis which sent the current time for 6 seconds. 

![image](https://github.com/user-attachments/assets/4f558205-f1ce-424d-b76e-ba0f939d0a0b)

I used the notification handler to receive these messages and print them to the Jupyter Notebook console. 

![image](https://github.com/user-attachments/assets/fa045aca-80eb-4dce-b1cd-dddb0b1832a9)

I collected 242 messages in 6 seconds, so the Artemis can send about 40.3 messages/second. 

Task 6: SEND_TIME_DATA

I programmed the Artemis to collect 20 timestamps in an array, and then send those timestamps to the computer by looping through the array. 

![image](https://github.com/user-attachments/assets/28ff837a-efba-4cdf-8013-4d3b412665ea)

I wrote this notification handler to parse the incoming data and append it to a list. This notification handler was used for Tasks 6 and 7, so it had more functionality than is strictly necessary. 

![image](https://github.com/user-attachments/assets/e0affb00-89cc-43ee-9215-fe093bd5cff5)

I tested out this message with the following Python code. 

![image](https://github.com/user-attachments/assets/8548af49-f13a-4acb-98d5-6c5dbbc6c19f)

Task 7: GET_TEMP_READINGS

This task was very similar to Task 6. I created another command in the Artemis that would append the current time and temperature for 20 iterations using the millis() and getTempDegF() commands into two separate lists. The Artemis would then loop through these lists and send the data to the computer. The Artemis code is shown below. 

![image](https://github.com/user-attachments/assets/8194a7fc-105a-4afc-9d50-5b0b62df5f78)

I used the same notification handler shown in task 6. It splits the incoming data into time and temperature data before appending them to separate lists. The same function is repeated below completeness. 

![image](https://github.com/user-attachments/assets/e0affb00-89cc-43ee-9215-fe093bd5cff5)

![image](https://github.com/user-attachments/assets/bdc4d78f-ebe7-4f41-9a15-a6404b705551)

Task 8: Method comparison

5000 Level Tasks:

Task 1: Effective Data Rate and Overhead

This task aimed to determine the data transfer rate for different-sized messages. The Artemis code processed an incoming message, created a message of a certain byte size, and sent it to the array. 

![image](https://github.com/user-attachments/assets/de9460ed-bd55-4751-9311-89eb01a96b06)

The corresponding Python code sends queries for different-sized messages and notes the send and return time. Using this, the data rate for each byte size is computed. This code and the resulting plot are shown below. 

![image](https://github.com/user-attachments/assets/0fd5a65b-c06b-4674-87b8-9dc7876b5e67)

![image](https://github.com/user-attachments/assets/43e4c40c-ec55-47bd-8821-f5e013f78048)

From the graph, we can see that the data rate is significantly higher when the message size is larger. From this, we can see that sending short packets of data introduces a significant amount of overhead, limiting the data rate. As an example, the data rate for 5-byte messages was 29.14 bytes/s and the data rate for 120-byte messages was 499.3 bytes/s. 

Task 2: Reliability

To test the reliability, I sent messages at different rates, comparing the number of sent messages with the number received. On the Artemis, I programmed it to collect 20 timestamps in a list, and then send that array to the computer. I varied the delay time when sending the command. The image shows when the delay was 100ms, but I tried it at 0, 10,50,100, and 200.

![image](https://github.com/user-attachments/assets/901e8dab-4518-4642-a014-a3d21f701fbc)

On the computer side, I wrote a notification handler that added the incoming data to a list and printed both the list and the length of the list. The screenshot shows that the computer successfully received all 20 messages when the delay was 100 ms. 

![image](https://github.com/user-attachments/assets/a1adb08c-e856-40df-9af8-c6da831f0861)

I repeated this same procedure for the delay times listed above. At each delay time, the computer received all the messages that the Artemis sent. From this, I can conclude that the BLE connection is very reliable. 

# Discussion

In this lab, I learned how to set up and send Bluetooth information between the Artemis and the computer. Additionally, this lab taught me how to embed videos and images inside of a github webpage. I also brushed up on C and Python coding skills, which will help me in future labs. 








