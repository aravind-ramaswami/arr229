# Lab 5: Linear PID control and Linear interpolation

In this lab, I implemented PID loop and used it to control my robot to a setpoint. 

# Prelab

In this lab, I developed a linear PID loop to enable my robot to stop at a given distance before the wall. I wrote python and Arduino code to easily send and receive data from the robot to the computer. To start, I wrote four more commands that would start the PID loop, stop the PID loop, set PID gains, and then send the relevant data to the computer. 

Start/Stop:
![image](https://github.com/user-attachments/assets/2029eb5c-5667-4e45-9a66-c85487b2b4af)

Set gains:
![image](https://github.com/user-attachments/assets/2589bea1-110f-49c4-89dd-494f3dfb4cf2)

Send data:
![image](https://github.com/user-attachments/assets/80a1b411-597f-4da1-85f6-ffc4e4b7ba52)

On the computer side, I wrote a callback function to extract the relevant data and store it in separate lists for further processing.

![image](https://github.com/user-attachments/assets/19e6453b-a70d-4db8-afcc-048712530cfa)

# Lab
The governing equation for PID control is:

![image](https://github.com/user-attachments/assets/0057a58f-da07-4433-aadf-8f5793e96067)

This controller is very versatile because the three separate gains allow the controller to be tuned for a wide range of circumstances. Additionally, PID does not require any knowledge of the system’s dynamics, making it a very general controller. 

# Position Control

I developed a full PID controller to achieve the best possible position control over my robot. I initially wanted to use the short mode distance (since it has the highest sampling rate) but quickly realized that it would not give accurate measurements at 2 and 3m. I had to use the long mode (20 Hz frequency) to get accurate distance measurements but sacrificed sensor speed. This was a necessary tradeoff because my robot had to work beyond 1 meter, exceeding the range of the short-mode distance sensor. I built my PID control in two separate functions.

One function (PID_Control) took the setpoint and the robot’s current position as inputs and returned a motor value between 0 and 1. The PID gains were scaled so the maximum output they would give was approximately 1 when the robot was at the maximum distance (starting position) from the wall, and zero when it was exactly at the desired position. Additionally, I accounted for the motor deadband region that I determined in Lab 4 so the PWM values were always between max and min, ensuring the robot would move with small PID gain values. I based this approach on Stephan Wagner’s and Anunth Ramaswami’s approach. I also added windup protection for the integrator term. This was tuned experimentally, but I caped the maximum integrator output at 20% of the maximum motor input. This prevents the integral term from dominating the control signal and driving the robot into the wall. I added a detivative term but did not need to implement a low pass filter or deal with a derivative kick. My performance with the raw derivative term is sufficient and I did not need to add anything more. 

![image](https://github.com/user-attachments/assets/3863b342-927b-464a-93fa-7597fe2994e1)

The 2nd function (drive_motors) took the motor value and direction as inputs and supplied the corresponding PWM signal to the motor. This function accounted for the deadband region of the robot’s wheels when computing the PWM value from the motor input. 

![image](https://github.com/user-attachments/assets/4c2c1dec-6099-4cb6-b6d5-ba298767fbe9)

The main loop code updates the robot’s position from the distance sensors and supplies this to the PID_control function. I automatically stopped the robot if it was within 10mm (tuned experimentally) of the target position to minimize oscillations and decrease settling time. 

![image](https://github.com/user-attachments/assets/f66fa6c4-3f0d-48e3-995f-2cc0eff5aa40)

I experimentally tuned my gains by setting the I and D terms to zero and only using P control. After P control was working, I added very small gains for I and D. The final gains I used were: kp = 0.00025, ki = 0.0000001, kd = 0.0001.

Here are several videos of robots working from different distances on a carpet surface.

Run 1:



Run 2:



Run 3:



Run 4:



Here is the corresponding data sent to the Jupyter Notebook for these runs. We can see that the maximum speed achieved on the last run was 1.417 m/s.

Run 1:

![t1_d](https://github.com/user-attachments/assets/b413bb75-a0b5-41c3-b0e3-5095fff20484)

![t1_c](https://github.com/user-attachments/assets/2e4347a8-a792-471a-8810-8453ce0f386e)

Run 2:

![t2_c](https://github.com/user-attachments/assets/e3472d97-df8e-4b5b-bf84-59f5717bf7b4)

![t2_d](https://github.com/user-attachments/assets/d7c77e4d-59b8-4401-b8f8-296866c436fb)

Run 3:

![t3_d](https://github.com/user-attachments/assets/9d12d4ff-9749-45a3-91b6-fe074349328a)

![t3_c](https://github.com/user-attachments/assets/f1fb60fd-183a-4be9-8aa8-1ed568c80d90)

Run 4:

![t4_d](https://github.com/user-attachments/assets/e2123c55-8481-4f84-a523-0cb72de4eaba)

![t4_c](https://github.com/user-attachments/assets/40d9826d-f98f-4962-a492-c8b28a684589)

The frequency of the TOF sensors was determined by computing the difference between successive time measurements (since they were synced to TOF values). I got a frequency of 20 Hz, which makes sense given long mode. This is slow, but my controller works well enough even with the low speed. 


# Extrapolation

From above, I measured the frequency of my PID loop at 20 Hz. I updated my PID loop to use the previous data if new sensor data was not ready. After this, the PID control and sensor input were decoupled. The new loop speed was 100 Hz, far faster than the original sensor speed. Next, I modified the code so it used the extrapolated data if the new sensor data was not ready. This shows the updated main loop code and linear extrapolation code. 

Main Loop:

![image](https://github.com/user-attachments/assets/52e02d81-e233-43ca-9cd7-5d08d6382d7c)

![image](https://github.com/user-attachments/assets/2eba3503-060a-4798-9694-0df830baca1b)

Linear Extrapolation:

![image](https://github.com/user-attachments/assets/50b4246f-8dd9-4493-bc24-607bcbb71366)

This video shows a successful run with linear extrapolation working. 

Video of it working:

Data:

![image](https://github.com/user-attachments/assets/a3a5f23e-8967-4c80-9d01-a3007f918941)

# 5000 Level Task





