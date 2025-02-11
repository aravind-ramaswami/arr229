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


# Gyroscope

# Sample Data

# Stunt


