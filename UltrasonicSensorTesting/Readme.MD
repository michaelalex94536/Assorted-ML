### Introduction

Practical, high-volume testing of ultrasonic range or time of flight (TOF) sensors presents challenges, in part due to the size and complexity of the hardware needed to measure the intensity of the reflected signal.  Here, we show how machine learning can be used to simplify test hardware, while at the same time allow one to quantify a key sensor parameter - the amplitude of the return signal reflected from a target.  

We first explain the basic principles of how ultrasonic range sensors work.  Then, we show a simple illustration of the transmitted and received ultrasonic signals and explain some of the key signal parameters of interest in characterizing sensor performance.  Finally, we walk through the analysis done here, where we used a simple Keras neural network model to predict the reflected signal amplitude using other, more readily-measured sensor parameters. 

### The basic operation of an ultrasonic range sensor

In Figure 1 we see a general overview of how a single transducer range sensor works [1].  (By single transducer we mean the acoustic signal is both launched from and detected by the same device.) A circuit in the sensor chip generates a start signal which causes a vibrating micro-electromechanical (MEMs) membrane in the transducer to emit a transmitted ultrasonic wave, the Tx signal.  Upon launching the Tx signal, the circuit is put into receive mode in order to detect the reflected wave from the target, the Rx signal. The Rx signal causes the same membrane that launched the Tx signal to vibrate, producing a voltage measured by read circuitry in the chip.  A timer in the chip counts the time between sending and receiving the Tx and Rx waves respectively, and knowing the speed of sound in the environment, the distance to the target is computed.  In present day ultrasonic range sensors, the vibrating membrane, the Tx and Rx circuitry, and the timer are all contained in a small, millimeter-sized package; the ultrasonic Tx and Rx signals are typically emitted from a small orifice measuring several hundred microns in diameter in the top of the package. 

Successful operation of the range sensor requires, of course, robust detection of the Rx signal.  In addition to the distance between the sensor and the target, the amplitude of the Rx signal is also a strong function of the strength of the Tx signal.  Furthermore, the device operating frequency and other characteristics of the device and its environment also determine the intensity of the Rx signal.  

![Signals](https://github.com/michaelalex94536/Assorted-ML/blob/main/UltrasonicSensorTesting/images/TI_Tx_Rx.jpg)



### Ultrasonic sensor Rx and Tx signals

In the next figure, we show some of the details of the Tx and Rx waves discussed above [2]. 

The Tx wave is generated or launched during the Excitation stage of device operation as seen in Figure 2.  A relatively high voltage square wave is applied to the MEMs membrane inside the sensor chip, causing the membrane to vibrate, which in turn causes the emission of an acoustic Pulse signal to be transmitted from the sensor.  This is depicted by the blue trace in the figure.  (The frequency of the acoustic wave is the same as the frequency of the Excitation voltage source square wave.) The Excitation square wave is applied for a relatively short time interval; the transmitted wave or Pulse signal is essentially a short burst or "wave packet" of ultrasonic energy.  

We see in the figure a finite time is required before the maximum voltage in the Excitation phase is reached.  Furthermore, even after the driving voltage is terminated at the end of the Excitation phase, the Pulse signal does not abruptly end; the transducer is in a "Ring-down" state during which the Pulse signal gradually dissipates.  The behavior seen is essentially that of a mechanical spring-mass-damper system.  Eventually, the Tx pulse is reflected and returns from a distant target; the Rx pulse or Echo signal is shown as the orange trace in the figure.  The time of flight (TOF) is the time difference between launch of the Pulse signal and receipt of the Echo signal.  We note that should the Echo pulse return prior to the end of the Ring-down phase, in the so-called Blind area shown in the figure, the time of flight can not be measured accurately.  To keep the Blind area as short as possible, the Excitation and Ring-down phases should be as short as possible, within practical limits.  

Looking at the transmitted and received pulses, we can see there aren't many parameters readily available in order to characterize the performance of the sensor.  Referring to the figure once again, these parameters are the Excitation voltage, the duration of the Ring-down state, the time of flight, as well as the Echo signal voltage.  Not readily discernible is the fact we also know the frequency of the transmitted pulse.  From the basic physics of operation of the device, these are the essential parameters that dictate ranging performance, but at the end of the day the key parameter is the Echo signal voltage.  No matter what the other parameters are (within reason), the Echo signal voltage determines if our reflected signal will be sufficient for robust and accurate target detection.  

Now, one must keep in mind ultrasonic MEMs TOF sensors like the one described above are capable of measuring distances of up to several meters.  This has significant implications for testing the devices, especially in high-volume manufacturing.  The test chambers and targets need to be large and bulky in order to facilitate the long-range testing needed to determine the maximum range of the device. Multiple devices can not be tested simultaneously in the same chamber using the same target due to crosstalk and interference from the other devices under test (DUTs). Furthermore, ultrasonic signals are notoriously hard to suppress and attenuate; reflections from the echo chamber walls and other test fixturing requires the test chamber be specially made in order to minimize undesired echos and spurious reflections that may interfere with accurate distance and Echo signal intensity measurements. Since the Echo or reflected signal intensity is the primary metric for determining sensor performance, it would be ideal if one could predict the intensity or amplitude of this signal without the complexities of using a large, complex echo chamber and target assembly.  

![Signals](https://github.com/michaelalex94536/Assorted-ML/blob/main/UltrasonicSensorTesting/images/pulse-echo.jpeg)
##### Figure 2  Illustration of the transmitted signal (Pulse signal in blue) and reflected signal (Echo signal in orange) of an ultrasonic range sensor. 
<br/><br/>

### The UltraSonicSensorAnalysis Jupyter notebook

Here we explain the "UltrasonicSensorAnalysis.ipynb" Jupyter notebook.  We first read-in a data file which has Frequency, Ring-down, Excitation, and Echo data for approximately 900 sensors tested in production. The neural network model features are Frequency, Ring-down, and Excitation, and our goal is to predict the Echo signal.  If the predictions are accurate, it means we are capable of estimating the sensor reflected signal amplitude without the need for bulky and inconvenient target and echo chamber hardware. 

The measurement data is randomly split up into training and test sets by Die ID, and we confirm there is no "crosstalk" between the training and test data.  The ratio of data used for training/testing can be selected by the user, where the default ratio is 75/25.  Then, a very simple neural network with two hidden layers is constructed in Keras, where each hidden layer is followed by a ReLu activation function. We see the default model has just under 100 parameters, so training can be done without need for a GPU. In Figure 3 we see the training and test loss versus the epoch number.  AT the end of training the test loss is slightly higher that the training loss.  Finally, in Figure 4 we show the predicted versus measured Echo signal amplitude.  The correlation is excellent, showing sensor performance can be accurately predicted using a small set of easily-measured test parameters. 

![Signals](https://github.com/michaelalex94536/Assorted-ML/blob/main/UltrasonicSensorTesting/images/training.jpg)
##### Figure 3  Loss for the training and test data sets vs. epoch. 

![Signals](https://github.com/michaelalex94536/Assorted-ML/blob/main/UltrasonicSensorTesting/images/predictions.jpg)
##### Figure 4  Predicted versus measured Echo signal intensity. The dashed red line shows the linear best fit line; the solid black line is the 1:1 regression.  



[1]  Texas Instruments Application Report SNAA220A, Ultrasonic Sensing Basics for Liquid Level Sensing, Flow Sensing, and Fluid Identification Applications, March 2015 

[2]  Sensors 2021, 21(19), 6414; https://doi.org/10.3390/s21196414 

