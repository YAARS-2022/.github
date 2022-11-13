# SahaYatri

## The problem SahaYatri solves
A huge number of parents, especially for smaller children, need to physically drop and pick their children from bus stops. But since the traffic in cities is very unpredictable, the arrival of school buses varies from day to day. As such, parents are left either continuously calling the bus driver or waiting for the bus at the bus stop for an unreasonable amount of time. A bigger problem arises when the bus is late and the driver's phone cannot be connected. In this case, parents will be worried about where their kids might be and even the school will get a lot of calls from worried parents.

Our project, SahaYatri, attempts to solve these problems using a low-powered GPS tracking device. Using our device and the software that comes along with it, parents can get real-time data about the location of their child’s bus. They will also receive a push notification when the child’s bus will come near the stop, and they can also get detailed information about the route and the bus if desired.

We also provide a school administrator version, which can be used to keep track of all the school buses. Here they can see if any of their buses are off track or if the buses are driven responsibly.

## Challenges we ran into

### Hardware issues:
For the sender device, we mainly used LoRa and GPS Module (Neo-6M). When we tested those devices individually, they worked as intended but when we tried to integrate them together we faced some issues:
- The operating voltage of LoRa is 3 - 5V and that of GPS Module is 2.7-3.6 V. Since a 3.3V line is readily available in arduino, we decided to use the same voltage rail to provide power to the devices. But as current got divided into two devices, LoRa couldn’t initialize due to insufficient power. So, we later powered LoRa using the 5V line and GPS module using the 3.3V line provided in arduino.
- LoRa uses SPI protocol for communication and GPS module uses UART. Since the serial ports for UART were needed for printing debug logs, we used SoftwareSerial in order to read data from GPS module. Since SoftwareSerial and SPI uses the same timer in arduino, we weren’t able to initialize both the devices at the same time. In order to solve it, in a loop, we initialized SoftwareSerial, saved the recieved data in a string and then closed the SoftwareSerial. After that, LoRa was initialized, and the saved string was sent to the reciever . The class was destructed so that SoftwareSerial can be reinitialized in next loop for reading data from GPS.

### Gradle issues:
When we tried to use a package for notification in the android app, there was an error while compiling the application. It took some time to find exactly which part of the code caused the problem. We had to edit the dependencies and use a certain version of AndroidX lifecycle view-model.

