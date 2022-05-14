<div id="top"></div>

# Robocar_SWP

## Table of contents
<ol>
    <li><a href="#linked-repositories">Linked repositories</a></li>
    <li><a href="#About-The-Project">About The Project</a></li>
    <li><a href="#System-breakdown">System breakdown</a></li>
    <li><a href="#Contact">Contact</a></li>
    <li><a href="#pictures_process">Some pictures during the process</a></li>
    <li><a href="#pending_tasks">Pending tasks / possible updates</a></li>
</ol>

## Linked repositories <a name="linked-repositories"></a>

Main Control System (ST Nucleo-F767ZI + FreeRTOS + C + motion/sensors drivers + SPI): https://github.com/juanma-rm/Robocar_car_stm <br>
Wifi System (Wemos D1 mini + C++ + WiFi + SPI): https://github.com/juanma-rm/Robocar_Wemos_SPIWifi <br>
Telemetry System (Python + Multiprocess + GUI + WiFi): https://github.com/juanma-rm/Robocar_control_app <br>

## About The Project <a name="About-The-Project"></a>

The result of this project is a remotely-operated two-wheeled robotic 3D-printed car that allows both manual and automatic (PID-based) control. The system is featured by an ST Nucleo board for main control, a ESP8266-based Wemos D1 mini board, two wheels for and two DC PWM-controlled motors for motion, two ultrasonics sensors for object detection, an idle ball wheel for stability and a 3D printed customized chassis.

This repository centralises the whole project

The component selection is not intended to be the best in terms of price, performance or even ease of design / implementation; the motivation of this project is to gather new knowledge and learn new technologies while enjoying the process.

## System breakdown <a name="System-breakdown"></a>

Motion and Power System:
* 2 x DC motors JGA25-371 DC12V500RPM featuring internal rotary encoder to provide angular speed feedback.
* 1 x L298N dual-channel H-bridge for controlling the previous motors via PWM technique. It also features a DC-DC step-down converter to provides 5V to the digital components from the 11.1V main battery.
* 2 x 65mm diameter wheels.
* 1 x idle ball wheel for car stability, located in the front part.
* 2 x HC-SR04 ultrasonics sensors to track close object distance.
* 3D customized chassis designed in AutoCAD (https://www.autodesk.es/products/autocad/overview). 
* 3 x 18650 battery cells to provide 11.1V and (theoretically) 2850 mAh. Shell for 3x18650 cells. Estimated needed current: 1.725 A = 2*0.6A (motors) + 300 mA (STM) + 170 mA (Wemos) + 15 mA (hcsr04) + 40 mA (l298n logic).
* Electronic design built with KiCAD (https://www.kicad.org/)

![image](https://user-images.githubusercontent.com/41286765/168441648-a5490601-740b-40cd-a0cd-d3f69b2856ff.png)

***Main Control System***: in charge of handling the direct control on the car hardware (motors, encoders and ultrasonics sensors). It receives external orders and provides feedback on the current status via SPI slave communication. 
* [ST NUCLEO-F767ZI](https://www.st.com/en/evaluation-tools/nucleo-f767zi.html), based on stm32f767zi (Arm® 32-bit Cortex®-M7 CPU, 512 KB SRAM) and 2MB Flash, among other features.
* Running on top of [FreeRTOS] (https://www.freertos.org/) + C language.
* Programmed with [STM32CUBEIDE] (https://www.st.com/en/development-tools/stm32cubeide.html)

***Wifi System***: adds WiFi capability to the Main System, simply interchanging data coming from the Telemetry System (via Wifi) and the Main System (via SPI, being the Wifi System the one mastering the communication).
* [Wemos D1 mini] (https://www.wemos.cc/en/latest/d1/d1_mini.html) (esp8266 based)
* Programmed in C++ around Arduino framework.

***Telemetry System***: Python-based GUI multithreaded application which allows sending orders to and receiving telemetry from the car via WiFi. Running on laptop.
* Possible commands to send to the car are specific percentages of speed in both straight and side directions or lineal speed setpoint for automatic control PID-based control (processed within the Main Control System). 
* A process is in charge of the WiFi server and another process handles the GUI, with queues as the mechanism to pass messages between they both.
* GUI based on PySimpleGUI (https://pysimplegui.readthedocs.io/en/latest/)
![Telemetry_System_GUI](https://user-images.githubusercontent.com/41286765/168441949-a883e276-1fc4-42ed-9194-7f1c5f434684.png)


## Some pictures during the process <a name="pictures_process"></a>

![image](https://user-images.githubusercontent.com/41286765/168442473-c7a252ac-f445-4122-b708-f35df00d4278.png)
<br>First generic approach to hardware diagram (some points still pending to be determined or finally discarded)

![image](https://user-images.githubusercontent.com/41286765/168442579-94f6c3af-3dee-4aa4-ae64-7d5dffdec458.png)
<br>Electronic schematic

![image](https://user-images.githubusercontent.com/41286765/168441883-f6aa4b75-e672-4a5d-beaa-f12ec8a540cb.png)
<br>Test of motor / PWM motion 

![image](https://user-images.githubusercontent.com/41286765/168442017-0e8bbd79-a174-4c71-896f-b4214bb3d8a8.png)
<br>Testing encoder signals on oscilloscope

## Pending tasks / possible updates <a name="pending_tasks"></a>

* Receive 3D chassis and build everyting + final test.
* Main Control System: some modules pending to be documented
* Main Control System: PID closed-loop control (estimate system frequency model, calculate best PI/PID parameters, test, ...)
* Replace Main Control System board (ST Nucleo) by a Xilinx Zynq-based, using baremetal / FreeRTOS / other OS and splitting the control between software (ARM in PS) and hardware (FPGA in PL).

## Contact <a name="Contact"></a>

[![LinkedIn][linkedin-shield]][linkedin-url]


<p align="right">(<a href="#top">back to top</a>)</p>

<!-- README built based on this nice template: https://github.com/othneildrew/Best-README-Template -->

<!-- MARKDOWN LINKS & IMAGES -->

[linkedin-shield]: https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white
[linkedin-url]: https://www.linkedin.com/in/juan-manuel-reina-mu%C3%B1oz-56329b130/
