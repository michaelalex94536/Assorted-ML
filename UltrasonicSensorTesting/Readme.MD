### Introduction

Practical, high volume testing of ultrasonic range or time of flight (ToF) sensors presents challenges.  Here, we show how machine learning was used to simplify test hardware while at the same time allow one to characterize a key sensor parameter - the amplitude of the return signal reflected from a target.  First, we need to explain the basic principles of how ultrasonic range sensors work.  

### The basic operation of an ultrasonic range sensor

In Figure 1 we see a general overview of how a single transducer range sensor works.  An ASIC or circuit in the sensor chip generates a signal which causes a vibrating membrane in the transducer to emit a transmitted ultrasonic wave, the Tx signal.  Upon launching the Tx signal, the circuit is put into receive mode in order to detect the reflected wave from the target, the Rx signal. The Rx signal causes the same membrane that launched the Tx signal to vibrate, producing a voltage that the circuit measures.  A timer in the chip counts the time between the Tx and Rx waves, and knowing the speed of sound in the environment, the distance to the target is computed.  In present day ultrasonic range sensors, the vibrating membrane, the Tx and Rx circuitry, as well as the timer are all contained in a small, millimeter-sized package; the ultrasonic Tx and Rx signals are emitted from a small orifice measuring several hundred microns in diameter in the top of the package. 



![Signals](https://github.com/michaelalex94536/Assorted-ML/blob/main/UltrasonicSensorTesting/images/TI_Tx_Rx.jpg)
