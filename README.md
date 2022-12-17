# Arduino-based central heating stove extension

This project integrates a stove into a natural gas-based central heating system. The basic idea of this project is to use excess heat from the stove to reduce the amount of natural gas used by the central heating system of a house. Another benefit is the fact, that the heat exchanger provides enough energy to heat the raw water tank to provide enough hot water for daily use including showering each day during winter season without using any gas. The system was able to provide enough heat for the entire house until early winter in northern germany.

A [smoke gas heat exchanger](https://www.lupi-waermetauscher.de/wp-content/uploads/2018/09/1a_AWT7_Schnitt_08_VL_S.jpg) is used to extract excess heat from the stove's chimney. After the smoke gas temperature reaches 80 °C, the pump starts circulating water through the heat exchanger increasing the raw water tank's temperature. If the limit of 60 °C is reached, the hot water from the heat exchanger is directed towards the central heating's forward flow. This homogenises the house's temperature distribution and is also used as a safe operating point in case any of the temperature measurements provide implausible readings. If the raw water tank's temperature decreases below 50 °C, the heat is used to heat the raw water again. [Two-State electric valves](https://m.media-amazon.com/images/I/61eKWHQV8CL._AC_SY355_.jpg) as well as [circulating pumps](https://www.wuh24.de/media/image/0f/f7/52/grundfos_96913060.jpg) are used to direct the flow. [PT1000 PTC Resistors](https://www.sensorshop24.de/media/catalog/product/cache/2a42d0df1a5e6b0a267b39a612f0dae1/d/6/d6_uebersicht.jpg) each combined with a $1.5~k\Omega$ resistor in series are used to measure the temperatures of smoke gas, raw water tank and central heating forward flow. The measurements are performed by an [Arduino Nano](https://store.arduino.cc/products/arduino-nano)'s internal ADC. The Nano is also used to calculate the temperatures as well as turning on the pumps and switching the valves using standard realy modules.

For safety reasons the system provides 3 operational modes:
* Auto: The system performs the described functions automatically.
* Manual: The circulating pumps are switched on, the valves may be switched manually.
* Off: The central heating forward flow circulating pump is controlled by the central heatings control system.

## System overview

![System Overview](/Pictures%20and%20Drawings/System%20overview.png)

Temperature Measurements:
* Smoke gas PT1000 (4-wire)
* Raw water PT1000 (2-wire)
* Forward flow central heating PT1000 (2-wire)
 
Valves
* Stove heat to Raw water
* Stove heat to Forward flow central heating
  
Circulating pumps
* Stove heat exchanger flow
* Forward flow central heating

# Software implementation

The code was written using Arduino IDE 1.8.19 and doesn't require any external libraries. After the setup and declaration of several constants the code consists of 8 parts which are designed to function just like standard programmable logic controllers (PLC):
1. readInput(); Read the current ADC input values from the Nano's analog input pins
2. calcR(): Calculating the resistances of the PTC resistors from the ADC readings
3. RtoT_PT1000(): Calculating the temperatures from the resistances of the PTC resistors using lookup table from -200 °C to +859 °C and using an exstimation function ot of those bounds
4. setMode(): Setting the operating mode depending on temperature readings
5. setOutput(mode): Set Outputs depending on the calculated operating mode
6. setActuatorsPower(): Set actuators power if the electric valces have to change status for 30 seconds
7. writeSerial(): Sending temperature readings and current operating mode to serial for debugging purposes
8. writeOutput(): Write the output values to the Nano's digital output pins

# Hardware implementation

Arduino Nano pin connection scheme:

| Arduino Nano Pin | Function | Connection |
| ----------- | -------- | ------ |
| D2 | Digital Output | Raw water valve |
| D3 | Digital Output | Central heating system forward flow valve |
| D4 | Digital Output | 30 sec energy for valves |
| D5 | Digital Output | Stove circulating pump |
| D6 | Digital Output | Forward flow heating circuit circulating pump |
| A0 | Analog Input | 2-wire-Raw water |
| A1 | Analog Input | 2-wire-Forward flow |
| A2 | Analog Input | 4-wire-Smoke gas (1) |
| A4 | Analog Input | 4-wire-Smoke gas (2) |
| A6 | Analog Input | 4-wire-Smoke gas (3) |

## Pictures
Pictures of the implementation can be seen below.

![Test Setup PCB](/Pictures%20and%20Drawings/Test_Setup_PCB.jpg)
![Test Setup with pipes](/Pictures%20and%20Drawings/Test_Setup_fully_functional_.jpg)

# License

This library is free software; you can redistribute it and/or
modify it under the terms of the GNU Affero General Public
License as published by the author; either
version 3 of the License, or (at your option) any later version.

This library is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
Affero General Public License for more details.

You should have received a copy of the GNU Lesser General Public
License along with this library; if not, feel free to contact the author.
