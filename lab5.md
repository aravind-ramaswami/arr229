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




