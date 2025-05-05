# LAB 11: Localization (Real)

In this lab, I implemented localization on a real robot. Iniially, I used a simulator to test out the given localization code and confirm that it was working. In this lab, we were provided with optimized localization code, so I used this instead of using my implementation from lab 10.

# Simulation

I tested the simulated code and obtained the following result for the bayes filter. This screen shows the simulated code running successfully. 

<img width="542" alt="fast_robots_l11s" src="https://github.com/user-attachments/assets/d29ce8ef-0bde-4501-8576-96121e5ed5f3" />

# Real

After confirming with the simulator, I moved onto implementing localization on the real robot. I implemented the given functions in python to connect the simulator to my physical robot. 

<img width="926" alt="image" src="https://github.com/user-attachments/assets/b493e757-2eb8-442b-b512-1b7cc848ad8a" />

This is the main python function that communicates with the robot. This function sends BLE commands to the robot to enable PID and start localizing. There is delays built in to ensure the commands are sent and processed by the artemis. The robo does one full spin, gathers data, and then sends back the angle and distance data to the computer. This function uses callbacks to proccess the incoming data from the robot. After the data is collected, the function returns this data to the main localization script. 

The callback functions are provided below. They are essentially the same callback functions used throughout the remaining labs, but are listed here as well. 

<img width="724" alt="image" src="https://github.com/user-attachments/assets/ab614b7b-7eb4-45fe-945b-3e86e8edaf7e" />

The arduino code is essentially the same as lab 9, but I modified it so it only does increments every 20 degrees instead of 15. Additionally, I didn't collect 4 measurements per angle, only one. 

<img width="884" alt="image" src="https://github.com/user-attachments/assets/673d7d4d-8b2f-4c96-8558-51b2f10be569" />
<img width="893" alt="image" src="https://github.com/user-attachments/assets/452086c1-13d4-451e-8027-bc1cce0afa38" />

Using this code and the python functions, I evaluated the localization in 4 places. 

(-3, -2);

<img width="518" alt="image" src="https://github.com/user-attachments/assets/cfe2667c-082f-40e4-a5f8-e6586657d5f5" />

<img width="742" alt="image" src="https://github.com/user-attachments/assets/eb1989c3-4f17-40c2-975a-e6544df65bf4" />

In this case, the localization was pretty accurate, but not exact. 

(5,-3):

<img width="325" alt="image" src="https://github.com/user-attachments/assets/11317785-2972-4e5e-a912-5c45ffa075e7" />
<img width="299" alt="image" src="https://github.com/user-attachments/assets/3e3f1e7c-d8df-40b8-bc1a-99dd8fd341fd" />

<img width="740" alt="image" src="https://github.com/user-attachments/assets/dfb4cd39-85fd-4a8a-ac2c-2fad15b9c68f" />

The localization was almost perfect in this case, indicating that the sensor measurements were very good at this point. 

(5,3):

<img width="310" alt="image" src="https://github.com/user-attachments/assets/a2c614dc-5278-4f73-bb87-8c9501b1d255" />

<img width="717" alt="image" src="https://github.com/user-attachments/assets/4fafd58d-08f2-4d60-aa66-60ba448c7ad2" />

The localization was relatively accurate here, but not as good as the previous locations.

(0,3):

<img width="295" alt="image" src="https://github.com/user-attachments/assets/f26f812a-0428-4a41-8d21-5eeca66a94c2" />
<img width="311" alt="image" src="https://github.com/user-attachments/assets/7895b5a6-aceb-407d-8739-34d7cc85701c" />

<img width="719" alt="image" src="https://github.com/user-attachments/assets/7762d5b1-034d-4690-91cb-89fcae24e7db" />


The localization was almost perfect in this case, indicating that the sensor measurements were very good at this point. 

Overall, the sensor measurements were very accurate at (0,3) and (5,-3), potentially becuase of numerous obstacles surrounding them, so the sensor is able to localize off of multiple objects. Also, it is interesting the note that the initial angle measured was often around 10 degrees, even though my robot started at 0 degrees. This is probably because my robot's DMP was not zoroed perfectly, so it was at a slight offset. Additionally, the robot did not turn to the exact angles (18 degrees instead of 20), which casued some error in the localization filter. Overall, it did a good job localizing the robot. 

# Conclusion

It was cool to see the robot able to succesfully localize using a bayes filter. I don't know if I will actually use this during lab 12, but it was still cool to implement localization on a physical hardware. 
