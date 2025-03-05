# LAB 4: Motors and Open Loop Control

In this lab, I added two motor drivers to the car, removed the existing control board, and integrated my Artemis and TOF sensors into the car. 

# Prelab

I used pins 0,1,4,5 to control the motor drivers. I used stranded wires since they are more resistant to breaking than solid core wires. I used separate batteries for the Artemis and motor drivers to maximize the available current for the motor drivers. Additionally, the motor drivers require a lot of current, so separate batteries for the Artemis and motor drivers will maximize the robot's lifetime. 

Wiring Diagram

![image](https://github.com/user-attachments/assets/1af26e42-4e35-4191-ac1b-62ae6792f475)

# Lab Tasks

# Power Supply & Oscilloscope

I soldered the input pins of both my motor drivers to the Artemis and connected the output pins to an oscilloscope and the power pins to an external power supply. For the power supply, I used 3.7 V as the input voltage, since the 850 mAh batteries are 3.7 V batteries. From the datasheet, I noted that the maximum steady current drawn when coupling both channels was 2.4 A. I set a limit of 1.6 A since I did not think the motor drivers would need the maximum current when running without a motor. 

I used this code to generate the PWM signals for one of my motor drivers. 

![image](https://github.com/user-attachments/assets/4c4ea7b9-5a8a-490a-a0ff-e9052809a79b)

This is the oscilloscope output.

<iframe width="560" height="315" src="https://www.youtube.com/embed/10rf_lWhFPM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Controlling Motors

I soldered the motor driver outputs into the motors, following my wiring diagram. I powered the motor drivers using the same external power supply with the current limit set to 2.4 A. 

I used one motor driver to control the motors on its side. The code and video are shown  below. 

![image](https://github.com/user-attachments/assets/7e6a1d6a-c5be-421e-b9e8-263b6c5bf656)

<iframe width="560" height="315" src="https://www.youtube.com/embed/JEH6hXHR7c0?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


I repeated this process for the 2nd motor driver (to confirm its functionality).

![image](https://github.com/user-attachments/assets/35221941-cd53-45e8-9b9b-1c834e91a1df)

<iframe width="560" height="315" src="https://www.youtube.com/embed/J3eVWfjWoRw?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


# Battery Power

I used the battery power to drive each motor driver separately. I used the same codes in the previous section since the only change was the power source. 

One side:

<iframe width="560" height="315" src="https://www.youtube.com/embed/1wa9TMYpU7A" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Other side:

<iframe width="560" height="315" src="https://www.youtube.com/embed/2iXlc_S5TB8?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Assemble Everything

I assembled everything into my car, using electrical tape and double-sided tape to secure my components. I decided to place TOF sensors on the front and back of the car. Additionally, I drilled an additional hole in the battery compartment so I could have both batteries in the same compartment. The image below shows the assembled car. 

![image](https://github.com/user-attachments/assets/1be88986-e4e1-4d9f-891a-206fb8110619)

![image](https://github.com/user-attachments/assets/249d782f-2f7a-4522-9022-463614cceef0)

![image](https://github.com/user-attachments/assets/97e9f146-35a5-4a33-88b9-932008b9531b)



After assembling everything, I programmed the car to drive forward for a few seconds and then stop. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/2iXlc_S5TB8?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# PWM Lower Limit

I placed the car on the lab table and slowly incremented the PWM values until the car barely started to move. For straight, I found that a minimum PWM of 70 was needed to move forward.

<iframe width="560" height="315" src="https://www.youtube.com/embed/2iXlc_S5TB8?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

For turning, I drove one side forward and the other side backward to execute a turn. I found that a value of 100 on the left motor and 90 on the right motor was necessary to just start turning. This was higher than going forward because the car has to overcome more friction to turn then go straight. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/atTqy-LJ62I" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Straight Line

My car initially did not move in a straight line, so I had to add a calibration factor (increased the right motor speed) to ensure it moved in a straight line. Here is the code and the video.

![image](https://github.com/user-attachments/assets/5b2c8f49-e282-43f5-ae77-cf75cf99f7ae)

<iframe width="560" height="315" src="https://www.youtube.com/embed/QUrOptUJL00?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Open Loop 

After getting the car to move in a straight line, I programmed an open-loop procedure for it to follow. I sent the command using ble in Python. 

![image](https://github.com/user-attachments/assets/783dfbb4-d460-4b94-bd53-2f7b98ad53ee)

<iframe width="560" height="315" src="https://www.youtube.com/embed/uPKTonFRb08?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# 5000 Level Tasks

# Analog Write

I measured the frequency of the analogWrite commands using an oscilloscope. I found that the frequency was 186Hz. This frequency was enough to run the motors (based on experimental observations). If the PWM signal was faster, you could achieve finer control over the car for high-speed maneuvers. 

![PXL_20250304_082301995](https://github.com/user-attachments/assets/b58c6d29-30a0-4a7e-9046-4197080c9aa2)

# PWM Limits revisited

I tried to determine the lowest PWM value for which the robot would continue to drive forward. I set an initial PWM signal to start the robot, let it run for 1.5 seconds, and then supplied a lower PWM signal. I lowered the value until the robot stopped. The lowest value I got was 60, which is higher than expected, but could be because of extra wheel friction, weaker motors, or a depleted battery. 

![image](https://github.com/user-attachments/assets/de390dcc-fffb-45e4-b504-d8db8cb3e3f1)

<iframe width="560" height="315" src="https://www.youtube.com/embed/2iXlc_S5TB8?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

