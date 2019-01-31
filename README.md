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
Coming soon...
