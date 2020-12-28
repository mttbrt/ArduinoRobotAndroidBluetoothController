# Report
The project consists of an *Android application which controls an Arduino controller* equipped with wheels and a proximity sensor. The communication between the app and the car takes place via bluetooth in the following ways:
- Android to Arduino: to move the car forward, backward, right, left or stop it.
- Arduino to Android: to report the distance between the robot and an obstacle placed in front of it.

Some relevant features of the project are:
- Smartphone sensors employed to control the robot
- Calibration of these sensors
- Recording a route and make the robot replicate it in the future
- Source code and circuit design for Android app and Arduino robot-car

## Arduino
Components used for the robot:
- 1 x Arduino UNO 2012
- 1 x L293D IC
- 1 x HC-05 Serial Bluetooth Module
- 1 x HC-SR04 Ultrasonic Sensor
- 2 x TT Motor DC 3V-12V

The robot sends through Bluetooth module the distance received (in cm) from ultrasonic sensor, and receives commands to move forward/backward, left/right or stop.

```/Arduino/``` folder contains the circuit diagram and .ino code.

## Android
### Application functionalities
It consists of 3 basic functionalities (the core of the application) plus 3 more features that significantly improve the project.
- **Controller**: control robot direction with the app. The Android application sends to the Arduino device one of 4 commands available: move forward, move backward, turn right and turn left. The user can choose whether to communicate with the hardware platform through a simple controller consisting of 4 buttons, or using the smartphone sensors. In the latter case, the default sensors (if available) are the proximity sensor (close > back, far > ahead) and the game rotation sensor (rotate the smartphone to the right to make the robot turn right and rotate the smartphone to the left to make the robot turn left, like a steering wheel).
- **Recording**: by entering the recording mode the signals emitted by the controller aimed to control the robot are recorded; when the path is completed the sequence of controls can be saved. Then it is possible to load it and re-execute it, therefore the Arduino device will complete the saved path independently without entering commands.
- **Sensors**: instead of providing one, or few, predefined settings for controlling the robot via sensors, it is up to the users to choose the combinations of sensors from those available in their smartphone. Therefore, each of the 4 basic commands can be associated to a different sensor. Each sensor has a range of values within the command is triggered and the sensor axis is valid. This is saved via preferences. Example: the user selects the accelerometer sensor to turn right, sets listening for the X axis and sets the validity range 25%-50%, therefore when the smartphone records a change for the accelerometer sensor, for the X axis, if the recorded value is between 25% and 50% of the range of values that the sensor can take, then the Arduino robot-car will turn right.
- **Calibration**: in addition to the previous feature. Each axis of each sensor can produce extremely different values, and often the values indicated as maximum range don't match the recorded ones. In order to address this issue, a sensor calibration system is provided in order to improve the user experience. The sensor controller has two states: paused and running. When the controller is paused the minimum and maximum values produced by the sensors are saved. Thus the percentage of the sensor range that will trigger the command will vary according to the minimum and maximum values recorded during this phase. Example: when a user pauses the calibration of the rotation sensor, and rotates the smartphone of 45 degrees to the left and 45 degrees to the right, a minimum and a maximum value will be recorded. The, when the user will be in running mode, the command will start if the values recorded by this sensor will be within the previously recorded range. If the user recalibrates it turning the smartphone 25 degrees to the left and 25 degrees to the right, the range will change and the command will be triggered when the smartphone is in a different position than before. Preferences are also used here.
- **Notification**: to increase the attention in case of obstacles in front of the Arduino device, a notification will be sent to the smartphone with a counter of the number of times an obstacle has been recorded at a distance of less than 10cm from the robot-car. Therefore, if the application is running in the background, the user is notified even though he is not using the app. Moreover, a reset button cancels notification if present, and resets the counter.
