# Arduino-based central heating stove extension

Integrates a stove into a central heating system. Reads temperature measurements from the central heating system and the stove to control valves and circulating pumps to heat raw water or/and heaters from the stove's smoke gas.

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
