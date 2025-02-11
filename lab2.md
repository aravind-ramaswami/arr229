# LAB 2: IMU

The purpose of this lab was to get acquainted with IMU. 

# Setting up the IMU

I connected the IMU to the Artemis using the QWIIC connectors.

![PXL_20250211_004745628](https://github.com/user-attachments/assets/08f4c46b-6c00-4a2f-9dc0-4c3ccf092e69)

After connecting the IMU to the Artemis, I ran the example code to test its functionality. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/ELP9Sbhi3_w" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

AD0_VAL = 1 in the example code, indicating that the ADR jumper on the back of the IMU has not been soldered together. This shows that the IMU's address is 0x69 and allows for SPI communication. If the jumper is soldered together, SPI communication is not possible. It should be 1 because our ADR jumper is not soldered together, and we want to use SPI communication.  

![PXL_20250211_004752363](https://github.com/user-attachments/assets/47667c23-2ae2-4e01-9f6c-de15d764a236)

I also added a visual signal that the IMU had successfully turned on (LED blinks three times)

<iframe width="560" height="315" src="https://www.youtube.com/embed/gODMkvEyXVw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


Using the example code, I played around with the IMU by rotating, flipping, and accelerating the board vertically. As the IMU accelerates, only the accelerometer in that direction changes and the gyroscope shows little change. Additionally, as the IMU flips, you can see the direction of the accelerometer change as different axis directions are aligned with gravity. As the IMU rotates, the accelerometer values constantly change while the gyroscope values are only non-zero while the IMU is actively rotating. If the IMU is stopped at a particular angle, the gyroscope will read small values, since it only tracks angular rates, not positions. This can all be seen in the video below, where the IMU is flipped, rotated, and accelerated. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/NzEttMiU5Tw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


# Accelerometer

# Calibration && Accuracy 

I analyzed the accelerometer by collecting data on the pitch and roll at {-90,0,90} degrees. I used the following equations in my code:

![image](https://github.com/user-attachments/assets/0124093a-40ff-4502-9777-e9b1b6f68059)

I collected this data:

| True Angle | Measured Accelerometer Roll | Measured Accelerometer Pitch |
|----------|----------|----------|
| -90 | -89.15 | -88.23 |
| 0 | -0.30 | -0.75 |
| 90 | 87.83 | 87.94 |

Using this data, I performed a two-point calibration as described here: https://learn.adafruit.com/calibrating-sensors/two-point-calibration

Roll:

Reference Range = 90 - (-90) = 180

Raw Range = 87.83 - (-89.15) = 176.98

Corrected Value = -90 + 1.017 * (Raw Value + 90)

Pitch:

Reference Range = 90 - (-90) = 180

Raw Range = 87.94 - (-88.23) = 176.17

Corrected Value = -90 + 1.0217 * (Raw Value + 90)

These equations will give calibrated sensor outputs given a raw value. Overall, the accelerometer is very accurate, as the reference ranges nearly match the raw range. Additionally, the accelerometer measurements at -90, 0, and 90 degrees are very close to the reference values, with a maximum error of less than 2.5 degrees. The accelerometer is slightly more accurate for roll measurements, which is reflected in the relative two-point calibration equations for roll and pitch. 

# Noisy Data Analysis 

I collected accelerometer data on the pitch and roll while moving it around to analyze the pitch and roll data. 

Raw Data:

![image](https://github.com/user-attachments/assets/9d4a0809-b760-4c93-bdc3-e93f46089c5e)

FFT:

![image](https://github.com/user-attachments/assets/7ca945e8-7d3b-492d-be26-34874d9eebc4)

![image](https://github.com/user-attachments/assets/ce100c03-42e3-4cd1-adfb-397d48e3a1a0)

From the FFT data, I saw that most of the amplitude is contained in the first few frequencies. I wanted to remove the higher frequencies to generate a very clean signal. To achieve this, I chose a cutoff frequency of 6Hz, which lets in the high-amplitude low frequencies while rejecting the noisy higher frequencies. 

For the low pass filter:

$$
\alpha = \frac{T}{T + RC}
$$

T is 1/sampling frequency and $RC = 2*\pi*f$. In my case, this gave an $\alpha = 0.045$. I implemented this in the code block shown below. The output of my filter is also shown below. 

![image](https://github.com/user-attachments/assets/6ddb9e9f-0d48-41cb-86d9-5ef611ccbc82)

![image](https://github.com/user-attachments/assets/3fc528c2-186d-488d-a71e-01b56d695dfe)

![image](https://github.com/user-attachments/assets/404285fc-b54e-46a4-90e5-59635a927d84)

The output shows that the low pass filter successfully reduced the noisy signal and filtered out spikes in the data. 

# Gyroscope

I converted the gyroscope measurements to pitch, roll, and yaw using the following equations.

$$
\theta_{gyro} = \theta_{gyro} + g_y * dt
$$

$$
\phi_{gyro} = \theta_{gyro} + g_x * dt
$$

$$
\psi_{gryo} = \psi_{gryo} + g_z * dt
$$

I implemented these equations with this code block:

![image](https://github.com/user-attachments/assets/1ebef313-9301-42bd-a526-6b8186a1170a)


I held the IMU at a fixed angle and recorded the accelerometer, filtered accelerometer, and gyroscope data. 

![image](https://github.com/user-attachments/assets/4357da70-fe01-4f7e-a318-da65d41545ba)

![image](https://github.com/user-attachments/assets/05a979f3-d19b-473a-86fc-896b784dd283)

![image](https://github.com/user-attachments/assets/7c1096ac-38c4-487f-893f-483331d16ec3)

From this data, we can see that the gyroscope has significantly less noise than the accelerometer, but also drifts over time. Additionally, the accelerometer gave more accurate readings than the gyroscope when the IMU was held at a constant orientation. Finally, the gyroscope will become more accurate as the sampling frequency increases because the gyroscope can get updated measurements faster. 

I implemented a complementarity filter to combine the accelerometer and gyroscope measurements into a single reliable signal. I chose $\gamma = 0.8$ since the accelerometer gave more accurate measurements. I used the following equations:

$$
\theta = (\theta + \theta_{gyro})*(1-\gamma) + \theta_a * \gamma
$$

$$
\phi = (\phi + \theta_{gyro})*(1-\gamma) + \phi_a * \gamma
$$

I implemented this filter with this code block:

![image](https://github.com/user-attachments/assets/28c5f642-63e4-4319-bb82-7c81ecd493aa)

This shows the results of my filter:

![image](https://github.com/user-attachments/assets/3ac70848-ff1f-4c35-8ecb-30258814df90)

![image](https://github.com/user-attachments/assets/830c0b18-eeb0-4e8a-9326-708b6740c3e8)

The filter removes the vibrations, drift, and spikes from the input signal. It also gives accurate measurements that are unaffected by sudden spikes or vibrations. Finally, the filter also works for both positive and negative angles, making it a useful filter for future use. 

# Sample Data

I removed all serial print statements to speed up the execution of the main loop. I also added flags to start and stop data recording. These flags were necessary because the Artemis main loop runs faster than the IMU can process data. After speeding up, the IMU data is at a rate of 362hz while the Artemis runs at 48 MHz, so the Artemis runs much faster than the IMU. 

![image](https://github.com/user-attachments/assets/b00061a6-4e63-4374-83f0-3ee93fc96a21)

In my main loop, I recorded data if the flag was set and the IMU had new data. Otherwise, the main loop kept executing. 

![image](https://github.com/user-attachments/assets/77ce2530-5462-4a0d-af72-ee529e4408fe)

I processed the IMU data in a separate function shown below. I stored the data in 3 float arrays (pitch, roll, yaw) so I could process each array separately. Additionally, I can perform separate operations on each array such as filters or transforms. I stored them as floats as a balance between accuracy and space. Additionally, I stored the final combined  data in 3 arrays. I did not store the separate accelerometer, filtered accelerometer data, and gyroscope data in separate arrays to save space. I stored these values as floats and updated them as new data came in. 

![image](https://github.com/user-attachments/assets/19d6b81a-7429-4abc-abed-a84afed6d43a)

The Artemis has 384 kiB of memory. With 3 arrays and 4 bits per float, each array can store up to 32000 floats per array. Since the IMU processes data at 362hz, the Artemis can store about 88.4 seconds of IMU data if the data is stored in 3 separate float arrays. This is the theoretical maximum, and the true value would be lower since the Artemis needs some memory for other processes. 

I stored time-stamped IMU data, as shown in this image. 

![image](https://github.com/user-attachments/assets/157001ca-c9d9-4526-9266-f8754422ac43)

I also recorded 5.1 seconds on IMU data, as seen in this image which shows the difference between starting and stopping time stamps is over 5 seconds. 

![image](https://github.com/user-attachments/assets/dfa7590a-281d-4a93-befd-09fdbe24d3d5)

![image](https://github.com/user-attachments/assets/5796161c-21b8-4397-b4a1-d4c3a47797db)

# Stunt

I recorded a stunt with my robot. 


