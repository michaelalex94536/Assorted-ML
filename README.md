### Assorted-ML

Here, I show some of the machine-learning related projects I have worked on. 

#### UltrasonicSensorTesting 

This works demonstrates how ultrasonic distance sensor performance can be predicted after significant simplification of the testing hardware.  Various parameters of ultrasonic sensors were measured in a manufacturing facility.  The measurements required the use of a test system in which the reflected signal intensity was measured after it bounced off a target positioned in a custom-made "echo" chamber.  The target and custom-made echo chamber are costly, take up significant space in a manufacturing clean room, and are difficult to fabricate and maintain.  We show that by using machine learning, we can predict the intensity of the reflected signal by using other metrics measured during testing, enabling us to predict sensor ranging performance without the need for the bulky, expensive echo chamber.  

We first explain the basics of how ultrasonic ranging or distance sensors work, the key transmitted and received signal parameters, and then finally walk through a Jupyter notebook in which a simple Keras neural network model was used to accurately predict final sensor performance.  
