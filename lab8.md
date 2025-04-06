# LAB 8: Stunts!

# Overview 
In this lab, I programmed my car to do a drift that followed the following requirements:

1. Start the robot at least 2m from a wall
2. Drive forward fast
2. Turn 180 degrees when the robot is within 3ft of the wall
3. Drive back across the starting line

# Drift

I implemented the drift stunt by combining code I developed from the previous labs. Steps 1 and 2 were accomplished using the Kalman Filter code developed in Lab 7. I disabled PID control so the robot would drive forward quickly, instead supplying a constant PWM value. 

I used code from lab 6 to turn the car 180 degrees. I modified the orientation PID controller so it would turn 180 degrees from its starting orientation. I modified the PID gains to minimize the time it takes during the drift. This led to significant overshoot, but it made the stunt execute faster and look cooler (in my opinion). After turning the car 180 degrees, I programmed it to drive back across the starting line. 

The screenshots below show the code and the associated functions. The main code runs in a loop, calling external functions as necessary. The external functions are essentially unchanged from previous labs, but are listed here for completeness. 

Main Loop code: 

![image](https://github.com/user-attachments/assets/16353c8f-d7db-4a15-8da6-16918d90001d)

![image](https://github.com/user-attachments/assets/4c50c417-20d1-4ad1-986b-eea728a971ee)

![image](https://github.com/user-attachments/assets/425e94cb-9255-4d45-909d-213877edd4a8)

External Functions:

![image](https://github.com/user-attachments/assets/0904b90e-906c-449a-a757-8c7d1cda97a1)

![image](https://github.com/user-attachments/assets/74c5e37b-2183-406c-b465-3b164653768d)

![image](https://github.com/user-attachments/assets/9ab2c764-afc9-46b9-ad90-f3fa730ac776)

![image](https://github.com/user-attachments/assets/993933fe-5e79-49a2-8032-136d60bdbb1f)

I transmitted the data back to the computer using the same callback functions developed in labs 5 and 6. 

# Demonstrations








   
