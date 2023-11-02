### Introduction

Practical, high volume testing of ultrasonic range or time of flight (TOF) sensors presents challenges.  Here, we show how machine learning was used to simplify test hardware while at the same time allow one to characterize a key sensor parameter - the amplitude of the return signal reflected from a target.  

We first explain the basic principles of how ultrasonic range sensors work.  Then, we show a simple example of the transmitted and received ultrasonic signals and explain some of the key signal parameters of interest in characterizing sensor performance.

### The basic operation of an ultrasonic range sensor

In Figure 1 we see a general overview of how a single transducer range sensor works [1].  An ASIC or circuit in the sensor chip generates a start signal which causes a vibrating membrane in the transducer to emit a transmitted ultrasonic wave, the Tx signal.  Upon launching the Tx signal, the circuit is put into receive mode in order to detect the reflected wave from the target, the Rx signal. The Rx signal causes the same membrane that launched the Tx signal to vibrate, producing a voltage measured by read circuitry in the chip.  A timer in the chip counts the time between sending and receiving the Tx and Rx waves respectively, and knowing the speed of sound in the environment, the distance to the target is computed.  In present day ultrasonic range sensors, the vibrating membrane, the Tx and Rx circuitry, as well as the timer are all contained in a small, millimeter-sized package; the ultrasonic Tx and Rx signals are typically emitted from a small orifice measuring several hundred microns in diameter in the top of the package. 

Successful operation of the range sensor requires, of course, robust detection of the Rx signal.  In addition to the distance between the sensor and the target, the amplitude of the Rx signal is a function of the strength of the Tx signal.  Furthermore, the device operating frequency and other characteristics of the device and its environment also determine the intensity of the Rx signal.  

![Signals](https://github.com/michaelalex94536/Assorted-ML/blob/main/UltrasonicSensorTesting/images/TI_Tx_Rx.jpg)



### Ultrasonic sensor Rx and Tx signals

In the next figure, we show some of the details of the Tx and Rx waves discussed above [2]. 

The Tx wave is generated or launched during the Excitation stage of device operation as seen in the figure.  A relatively high voltage square wave is applied to a membrane inside the sensor chip, causing the membrane to vibrate, which in turn causes the emission of an acoustic Pulse signal to be transmitted from the sensor.  This is depicted by the blue trace in the figure.  (The frequency of the acoustic wave is the same as the frequency of the Excitation voltage source square wave.) The Excitation square wave is applied for a relatively short time interval; the transmitted wave or Pulse signal is essentially a short burst of ultrasonic energy.  

We see in the figure a finite time is required before the maximum voltage in the Excitation phase is reached.  Furthermore, even after the driving voltage is terminated at the end of the Excitation phase, the Pulse signal does not abruptly end; the transducer is in a "Ring-down" state and the Pulse signal gradually dissipates.  The behavior seen is essentially that of a mechanical spring-mass-damper system.  Eventually, the Tx pulse is reflected and returns from a distant target; the Rx pulse or Echo signal is shown as the orange trace in the figure.  The time of flight (TOF) is the time difference between launch of the Pulse signal and receipt of the Echo signal.  We note that should the Echo pulse return prior to the end of the Ring-down phase - in the so-called Blind area shown in the figure, the time of flight can not be measured accurately.  To keep the Blind area as short as possible, the Excitation and Ring-down phases should be as short as possible, within practical limits.  

Looking at the transmitted and received pulses, we can see there aren't many parameters available in order to characterize the performance of the sensor.  Referring to the figure once again, these are the Excitation voltage, the duration of the Ring-down state, the time of flight, as well as the Echo signal voltage.  Not readily discernible is the fact we also know the frequency of the transmitted pulse.  From the basic physics of operation of the device, these are the essential parameters that dictate ranging performance, but at the end of the day the key parameter is the Echo signal voltage.  No matter what the other parameters are (within reason), the Echo signal voltage determines if our reflected signal will be sufficient for robust and accurate target detection.  

Now, one must keep in mind ultrasonic MEMs TOF sensors like the one described above are capable of measuring distances of up to several meters.  This has significant implications for testing the devices, especially in high-volume manufacturing.  The test chambers and targets need to be large and bulky in order to facilitate the long-range testing needed to determine the maximum range the device is capable of. Multiple devices can not be tested simultaneously in the same chamber using the same target due to crosstalk and interference from the other devices under test (DUTs). Furthermore, ultrasonic signals are notoriously hard to suppress and attenuate; reflections from the echo chamber walls and other test fixturing requires the test chamber be custom-designed and made in order to minimize undesired echos and reflections that may interfere with accurate distance measurements. Overall, it would be ideal if one could estimate the



![Signals](https://github.com/michaelalex94536/Assorted-ML/blob/main/UltrasonicSensorTesting/images/pulse-echo.jpeg)
Figure 2  Illustration of the transmitted signal (Pulse signal) and reflected signal (Echo signal) of an ultrasonic range sensor. 


[1] Texas Instruments Application Report SNAA220A, Ultrasonic Sensing Basics for Liquid Level Sensing, Flow Sensing, and Fluid Identification Applications, March 2015 

[2]  Sensors 2021, 21(19), 6414; https://doi.org/10.3390/s21196414 

