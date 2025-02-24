# LAB 3: TOF

The purpose of this lab was to get acquainted with the TOF sensors

# Prelab

From the TOF datasheet, the i2c address was 0x52. I plan to use both TOF sensors simultaneously, so I will need to change the i2c address of one of them. I wired the XSHUT pin to A3 for one of the TOF sensors. Upon startup, I will temporarily disable this TOF sensor, assign a new i2c address, and enable it. Using both sensors simultaneously is ideal because it lets you capture data from different directions without turning, enabling better obstacle avoidance and path planning. 

Currently, I plan to place one i2c sensor on the front and one i2c sensor on the right side of the robot. With sensors on the front and side, the robot will detect obstacles in front and on the side. This could avoid colliding with walls when making turns. This sensor configuration will miss obstacles behind the robot, so I will be careful when backing up. Overall, I think having sensors facing two different directions will give the most information to the robot. 

Here is a sketch of my wiring diagram. I used the two long connectors to the TOF sensors, ensuring I could place them anywhere on the robot. I used the short connector to connect the sensors to the Artemis. 

![image](https://github.com/user-attachments/assets/24f8e27f-46cc-4d0c-abe6-355124e38a58)

# Lab Tasks 

# TOF Sensor Connections

This shows the hardware connections for the TOF sensors to the QWIIC breakout board. It also shows the battery connected to the Artemis.

![PXL_20250218_204834579](https://github.com/user-attachments/assets/e4b174f3-69d6-40ad-9f4e-cc34e2169ea8)

# i2c connection

![image](https://github.com/user-attachments/assets/750244fc-d572-42f2-9d72-caebfe939504)

The TOF sensor was listed as 0x29 when I expected it to be 0x52. Upon further inspection, I realized that the i2c protocol uses the last digit (LSB) as the read/write bit. As a result, the original address (0x52) is bit shifted to the right, yielding 0x29. 

# TOF Sensor Mode

I initially read through the dataset to investigate the three different sensor modes. However, the Arduino library only supports two sensor modes (short, and long), so I focused on those. The long mode is good up to 4 meters but is sensitive to lighting conditions. The short mode is only good up to 1.3 meters, but is robust to noise. Since 1.3 meters is about 4 feet, this mode will give enough distances for the vast majority of tasks I have to complete, so I chose to set both sensors on short mode to make them robust to noisy conditions. I tested the sensor's range, accuracy, and reliability at various distances. I recorded 40 sensor measurements at each distance and sent them to the computer via Python. The following graphs show my analysis:

![image](https://github.com/user-attachments/assets/8306f012-066d-413b-b9ab-2f5689e1ddc2)

![image](https://github.com/user-attachments/assets/5703794e-2ccd-4337-ad8b-0452dbb3f58a)

![image](https://github.com/user-attachments/assets/f88bb1d1-6de9-41e1-a915-0459d39c09b2)

These graphs demonstrate that the sensor is accurate from 25mm to 1000mm, slightly shorter than the datasheet's range of 1.3m. It had an accuracy between 10 - 20mm, which is higher than what the datasheet claims, but is probably affected by the errors in the testing setup as well. Overall, an accuracy of about 2cm is probably good for most of the required tasks. The sensors are very reliable, with a standard deviation of about 2mm. Near the end of the range, the accuracy decreases from about 15mm to 50mm. The reliability also decreases after about 1000mm, indicating that this sensor practically cannot be used for distances larger than a meter away, which is plenty of range for my tasks. 

# 2 TOF Sensors + IMU

I modified the code so I could use two TOF sensors simultaneously. I used the following routine to configure separate i2c addresses for each sensor. 

![image](https://github.com/user-attachments/assets/255965e8-0cda-4f9c-9e56-480610824621)

Using this, I successfully ran both sensors and printed their output to the screen, as shown in the following video. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/U3t2PjzP7MU?" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# TOF Sensor Speed

After testing out both sensors, I modified the Arduino code so the sensors could run as fast as possible. I printed the clock to Serial to analyze the Artemis loop speed compared to the TOF speed. I used this code:

![image](https://github.com/user-attachments/assets/097ee00d-7657-4d43-8f0b-197a0974a345)

This image shows the Serial monitor while running this code. 

![Screenshot 2025-02-22 161109](https://github.com/user-attachments/assets/59627cf9-3229-4788-9b17-59cbf7593c4d)

From the screenshot, we can see that the Artemis loop runs every 13ms which is a frequency of about 76 Hz. The sensors run about every 98ms which is a frequency of 10.2 Hz. The sensors run slower than the loop, so they are the limiting factor when running this code. 

# Time vs Distance

I modified the lab 2 code to incorporate the TOF sensors and the IMU together. I wrote a new case statement that collects TOF and IMU data for a set period, and sends this data to the computer. This is the code I used. It is the same IMU code used in the lab, modified to use the TOF sensors. 

![image](https://github.com/user-attachments/assets/0887f2b8-2d56-477d-bddc-98c056b55c6e)

![image](https://github.com/user-attachments/assets/0f7c8f22-e30e-43ac-bc37-772fdb0a86df)

This shows the callback function on the Python side. 

![image](https://github.com/user-attachments/assets/8a4a27c5-ca48-42f7-b58c-9dad90e3fc71)

This graph shows the two TOF sensors' output over time. 

![image](https://github.com/user-attachments/assets/17a49d87-ab96-4b84-a201-53480bebc22f)


# Time vs Angle

This graph shows the IMU data over time. It was collected from the code described in the previous section. 

![image](https://github.com/user-attachments/assets/07329220-7543-46d6-a306-7447ccda84bf)

# 5000 Levels Tasks

# IR-based Sensors

There are several different types of IR distance sensors. There are amplitude-based IR sensors, triangulation IR sensors, and IR TOF sensors. Amplitude-based IR sensors work by sending out IR pulses and comparing the amplitude of the sent signal to the received signal. From the ratio, the sensor can determine the distance of objects. They are very cheap and have a sampling rate, but they only work over short distances. Additionally, they are sensitive to ambient light, texture, and object color. Triangulation IR sensors work by sending out an IR pulse at a specific angle and measuring the angle of the received pulse. Based on this angle, the sensor computes the distance. These sensors are good up to 1m and are insensitive to surface texture or color, but they are bulky, expensive, and slow. IR TOF sensors work by sending out a pulse and measuring the return time. The distance is computed based on the return time since the speed of light is known. These sensors are not sensitive to ambient light, surface texture, or color, but have a slow operating speed. Finally, there are LIDAR sensors, which send out pulses of light and record how long it takes the pulse to return. They can generate very detailed maps but are very expensive and bulky. 

# TOF Sensor Sensitivity to Color/Textures

I tested out the TOF sensors for objects of different colors. I evaluated my sensor against blue, black, and white. Note, in the graph, the "red" marker symbolizes white. I measured 3 distances (averaged 40 values for each one) for each color and plotted them against each other and the true distance. 


From the plot, 


