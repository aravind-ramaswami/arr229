# LAB 8: Stunts!

# Overview 
In this lab, I programmed my car to do a drift that followed the following requirements:

1. Start the robot at least 2m from a wall
2. Drive forward fast
2. Turn 180 degrees when the robot is within 3ft of the wall
3. Drive back across the starting line

# Drift

I implemented the drift stunt by combining code I developed from the previous labs. Steps 1 and 2 were accomplished using the Kalman Filter code developed in Lab 7. I disabled PID control so the robot would drive forward quickly, instead supplying a constant PWM value. 

I used code from lab 6 to turn the car 180 degrees. I modified the orientation PID controller so it would turn 180 degrees from its starting orientation. I modified the PID gains to minimize the time it takes during the drift. This led to significant overshoot, but it made the stunt execute faster and look cooler (in my opinion). After turning the car 180 degrees, I programmed it to drive back across the starting line. One major modification I made was that I slowed down the DMP's data rate, so it would overload and crash the Artemis. I implemented this after reading Stephan Wagner's website, where he first implements this to prevent the DMP from crashing his Artemis. 

The screenshots below show the code and the associated functions. The main code runs in a loop, calling external functions as necessary. The external functions are essentially unchanged from previous labs, but are listed here for completeness. 

Main Loop code: 

![image](https://github.com/user-attachments/assets/16353c8f-d7db-4a15-8da6-16918d90001d)

![image](https://github.com/user-attachments/assets/ad16c594-480c-4135-bb2a-c48aa7c84537)

![image](https://github.com/user-attachments/assets/d50e36b8-40c3-480f-a8f3-d1361afd492d)

External Functions:

![image](https://github.com/user-attachments/assets/0904b90e-906c-449a-a757-8c7d1cda97a1)

![image](https://github.com/user-attachments/assets/74c5e37b-2183-406c-b465-3b164653768d)

![image](https://github.com/user-attachments/assets/9ab2c764-afc9-46b9-ad90-f3fa730ac776)

![image](https://github.com/user-attachments/assets/993933fe-5e79-49a2-8032-136d60bdbb1f)

I transmitted the data back to the computer using the same callback functions developed in labs 5 and 6. 

# Demonstration

I recorded 3 videos of the robot successfully executing the stunt. 

Trial 1: starting distance = 2184mm , run time = 4s

video: <iframe width="560" height="315" src="https://www.youtube.com/embed/sDajvFeTYnI?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

![t1_d](https://github.com/user-attachments/assets/b3f377cf-377b-4976-b771-eef6b24675f7)
![t1_y](https://github.com/user-attachments/assets/5b17ecd9-3dac-49c8-b59a-f6cc28b5cafa)
![t1_m](https://github.com/user-attachments/assets/cf253504-30ae-48cc-8b97-ad239dd7875c)

Trial 2: starting distance = 2205mm, run time = 

video: <iframe width="560" height="315" src="https://www.youtube.com/embed/bo1iYB6LCBQ?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

![t2_d](https://github.com/user-attachments/assets/2d5d026d-416f-4b16-8410-da56b8d2be8b)
![t2_y](https://github.com/user-attachments/assets/c834c542-b10a-4bd1-b3b5-abe3f2e74322)
![t2_m](https://github.com/user-attachments/assets/2ce23f25-bcc7-4a4a-a4b4-fcd32d9bbfab)

Trial 3: starting distance = 2401mm, run time = 

video: <iframe width="560" height="315" src="https://www.youtube.com/embed/jF0804UjzqE?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

![t3_d](https://github.com/user-attachments/assets/c882cfca-0be8-4142-9add-3ebf39cb2659)
![t3_y](https://github.com/user-attachments/assets/57c62413-5494-472e-a1b5-bc35eb136b8d)
![t3_m](https://github.com/user-attachments/assets/f2de459e-2f1c-416a-bf0f-76b0833c5f75)

I have added videos of several successful stunts to showcase system reliability. 











   
