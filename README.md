# Motorized Kinematic Mount (MKM)
Here we present a simple, flexible and cost-effective system that combines 3D printed components and an Arduino board to automatize a vast majority of kinematic mounts available on the market.

<p align="center">
  <img src="images/figure1.png" width="320"/>
  <figcaption><bolf>Figure 1. Fully assembled Motorized Kinematic Mount.</bold></figcaption>
</p>

The simplicity of the system relies on the components used to implement it. As show in the next figure, the system is composed of a 3D printed plate that supports two stepper motors 28BYJ-48 whose shafts are aligned with the knobs used to tip and tilt the plane of the kinematic mount that supports the optics. The cylindrical adapter used to couple the shaft to the knob is also 3D printed. To control the motors, an Arduino MEGA connected to a sensor shield is used. 

Regarding flexibility, the hardware and software of the system can be easily modified according to the user needs. From the hardware side, since the 3D printed components that couple the system to the kinematic mount were parametrically designed using FreeCAD 0.16, the system is highly tunable by changing a few parameters on a spreadsheet. Similarly from the software side, the Arduino code is well documented and some usage examples are provided in the *.ino file. 

Last but not least, the cost of the system is directly related to its simplicity. As a reference, the total cost of fabricating a system composed of four stepper motors that can be used to control two kinematic mounts is around 50 EUR. As a result the system presented in this letter compares favorably with respect to commercial alternatives that can be roughly between 5 to 10 times more expensive. However, a word of caution should be added here. Since the system keeps track of the number of steps given by the stepper motor of each channel to estimate its current position, some inaccuracies may appear due to the hysteresis present on the stepper motors. Therefore, the usefulness of the system presented in this letter depends on the application. Fortunately, from our experience in the laboratory, we have found that the system is suitable for a wealth of applications ranging from teaching to research.

## Materials
The following table sumarizes the list of materials required to motorize a single Kinematic Mount.

## Hardware Implementation
A single unit of the system, capable of controlling the tip and tilt of a kinematic mount, is composed of two stepper motors (28BYJ-48) attached to a 3D printed plaque that aligns the knobs of the kinematic mount with the axis of the motors. The axis are coupled to the knobs using a 3D printed knob adapter. The motor drivers are connected to an Arduino MEGA sensor shield (attached to and Arduino MEGA) using Dupont wire jumper cables. 

<p align="center">
  <img src="images/figure2.png" width="320"/>
  <figcaption><bolf>Figure 2. Example of implementation for a Radiant Dyes kinematic mount.</bold></figcaption>
</p>

Regarding the motor used, the 28BYJ-48 is a very cheap stepper motor that requires five wires for its connection to the motor driver circuit (typically an ULN2003 is used). The motor can operate in the voltage range 5V to 12V and gives 4096 steps per one turn. This number comes from the fact that the internal stepper motor that requires 64 steps to give a complete turn is connected to a set of gears that provide a reduction ratio of 1/64. As a result the 28BYJ-48 requires 64x64=4096 steps to complete a full turn. 

<p align="center">
  <img src="images/figure3.png" width="640"/>
  <figcaption><bold>Figure 3. Motorized Kinematic Mount connection diagram</bold></figcaption>
</p>

Figure 3(a) provides a connection diagram for a single unit that requires two channels (tip and tilt). Each channel is composed of a stepper motor 28BYJ-48, driver electronics and a Dupont cable patch with six wires. Given the number of output pins available on the Arduino sensor shield, the board can be used to control up to five kinematic mounts simultaneously as shown in Figure 3(b).

Since the 3D printed components that couple the stepper motors to the kinematic mount were designed using FreeCAD 0.16, the distance between motor axis on the plate, and the inner diameter and height on the knob adapters can be easily modified using a spreadsheet. To attach the motors to the PLA plate, four M4 screws are required, whereas two M3 grub screws are used to secure the knobs to the knob adapters.

<p align="center">
  <img src="images/figure4.png" width="640"/>
</p>

The following table provides information on the parameters used for customizing the plate and the knob adapter to fit into four different types of kinematic mounts. 

## Software Implementation
In order to provide flexibility and simplicity from the software perspective, each motor can be controlled independently by sending a simple instruction defined by a command table through the serial port. A command table is a dictionary of instructions that can be divided into two categories: commands and queries. The first category corresponds to a direct order such as "move stepper motor connected to port 3, half-turn". The second type of command is used to ask information to the device about its status; for example the state of a digital variable, the value of a given counter or the identification string that contains relevant information about the device and its manufacturer. 

TABLE

For reference, table \ref{tbl:command_table} shows the complete list of commands used to control and retrieve information of the status of each motor connected to the Arduino board. Since the status of each motor is determined by a variable that keeps track of the number of steps given by the motor, it is assumed a one-to-one correspondence between the number of steps defined by software and the number of steps given by the axis of the motor. A step on the clock-wise direction adds one unit to the counter; a step on the counter-clock-wise direction subtracts a unit. To keep the current status of each motor over time, each counter is stored in the non volatile section of the Arduino (EEPROM memory) so that all the information is available even though the Arduino board is powered off.

Since the control method is based on a command table, there are many alternatives to implement the software interface that controls the system. Depending on the application, the user may require to perform simple actions driven by simple commands, or a more elaborate sequence of tasks dependent on external signals or defined by an algorithm with temporal dependence. The former can be implemented by sending commands through the serial port using a serial communication terminal compatible with the RS232 standard such as Termite [reference] and the latter can be implemented using MATLAB or Python.
