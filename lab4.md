# LAB 4: Motors and Open Loop Control

In this lab, I added two motor drivers to the car, removed the existing control board, and integrated my Artemis and TOF sensors into the car. 

# Prelab

I used pins 0,1,4,5 to control the motor drivers. I used stranded wires since they are more resistant to breaking than solid core wires. I used separate batteries for the Artemis and motor drivers to maximize the available current for the motor drivers. Additionally, the motor drivers require a lot of current, so separate batteries for the Artemis and motor drivers will maximize the robot's lifetime. 

Wiring Diagram

--Insert Wiring Diagram--

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

# Controlling Motors

I soldered the motor driver outputs into the motors, following my wiring diagram. I powered the motor drivers using the same external power supply with the current limit set to 2.4 A. 

I used one motor driver to control the motors on its side. The code and video are shown  below. 

![image](https://github.com/user-attachments/assets/7e6a1d6a-c5be-421e-b9e8-263b6c5bf656)

-- Insert Video--

I repeated this process for the 2nd motor driver (to confirm its functionality).

![image](https://github.com/user-attachments/assets/35221941-cd53-45e8-9b9b-1c834e91a1df)

--Insert Video--

# Battery Power

I used the battery power to drive each motor driver separately. I used the same codes in the previous section since the only change was the power source. 

One side:

--Insert Video--

Other side:

--Insert Video--

# Assemble Everything

I assembled everything into my car, using electrical tape and double-sided tape to secure my components. I decided to place TOF sensors on the front and back of the car. Additionally, I drilled an additional hole in the battery compartment so I could have both batteries in the same compartment. The image below shows the assembled car. 

-- Insert Labeled Images--

After assembling everything, I programmed the car to drive forward for a few seconds and then stop. 

-- Insert Video--

# PWM Lower Limit

# Straight Line

# Open Loop 

# 5000 Level Tasks

# Analog Write

# PWM Limits revisted


