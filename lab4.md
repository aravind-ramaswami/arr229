# LAB 4: Motors and Open Loop Control

In this lab, I added two motor drivers to the car, removed the existing control board, and integrated my Artemis and TOF sensors into the car. 

# Prelab

# Lab Tasks

# Power Supply & Oscilloscope

I soldered the input pins of both my motor drivers to the Artemis and connected the output pins to an oscilloscope and the power pins to an external power supply. For the power supply, I used 3.7 V as the input voltage, since the 850 mAh batteries are 3.7 V batteries. From the datasheet, I noted that the maximum steady current drawn when coupling both channels was 2.4 A. I set a limit of 1.6 A since I did not think the motor drivers would need the maximum current when running without a motor. 

I used this code to generate the PWM signals for one of my motor drivers. 

![image](https://github.com/user-attachments/assets/4c4ea7b9-5a8a-490a-a0ff-e9052809a79b)

This is the oscilloscope output.

--Insert Video--

I repeated this for the 2nd motor driver with the following code and oscilloscope output.

![image](https://github.com/user-attachments/assets/206cf853-1e87-450f-8345-e3bb37ad2b68)

--Insert Video--

