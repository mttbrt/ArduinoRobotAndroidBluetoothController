# Report
The project consists of an *Android mobile application which controls an Arduino device* equipped with wheels and proximity sensor. Communication is via bluetooth in two ways:
- Android > Arduino: to make it move forward, backward, right, left or stop it.
- Arduino > Android: to display the distance of the robot from an obstacle placed in front of it.

Some features are:
- Use of smartphone sensors to control the robot
- Calibration of these sensors
- The option of recording a route to make it run to the robot later
- A detailed development of both hardware and software

## Arduino
Components used in the robot:
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
- **Controller**: control robot direction with the app. The Android application sends to the Arduino device one of 4 commands: proceed forward, turn back, turn right and turn left. The user can choose whether to communicate with the hardware platform through a manual controller, consisting of 4 simple buttons, or through the use of smartphone sensors. In the latter case, the default sensors (if available) are the proximity sensor (close > back, far > ahead) and the game rotation sensor (turn the smartphone to the right to turn right the robot, and to the left to turn left the robot).
- **Recording**: by entering the recording mode the signals emitted by the controller to control the device are recorded; when the path is completed the sequence of controls can be saved. Then it is possible to load it and re-execute it, in this way the Arduino device will complete the saved path independently without entering commands.
- **Sensors**: instead of providing one, or few, predefined settings for controlling the robot via sensors, it is up to the users to choose the combinations of sensors from those supported by their smartphone. Therefore, each of the 4 basic commands can be associated to a different sensor. Each sensor has a range of values within the command is triggered and the sensor axis is valid. This is saved via preferences. Example: the user chooses the accelerometer sensor to turn right, sets listening for the X axis and sets the validity range 25% -50%, doing so when the smartphone records a change for the accelerometer sensor, for the X axis, if the recorded value is between 25% and 50% of the range of values that this sensor can take, then the right turn command will be sent to the Arduino device.
- **Calibration**: in addition to the previous feature. Each axis of each sensor can produce extremely different values, and often the values indicated as maximum range don't match the recorded ones. To solve this issue, is provided a sensor calibration system to improve the user experience. The sensor controller has two states: paused and running, when the controller is paused the values produced by the sensors are listened, the minimum and the maximum values recorded are saved. Hence the percentage of the sensor range that will trigger the command will vary according to the minimum and maximum values recorded during this phase. Example: a user pauses to calibrate the rotation sensor, rotates the smartphone 45° to the left and 45° to the right, this will produce a minimum and a maximum value, creating a range. When it is running, the command will start if the values issued by this sensor will be valid on the previously set range. If the user recalibrates it to 25° to the left and 25° to the right, the range will change and the command will be triggered when the smartphone is in a different position than before. Preferences are also used here.
- **Notification**: to increase the attention in case of obstacles in front of the Arduino device, a notification will be sent to the smartphone with a counter of the number of times an obstacle has been recorded less than 10cm in front of the robot. Therefore, if the application is active in the background, the user is notified even though he is not using the app. Moreover, a reset button cancels notification if present, and resets the counter.
