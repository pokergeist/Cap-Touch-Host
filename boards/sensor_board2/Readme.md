# Cap Touch Sensor Board

## The Sensor Board

The sensor board is similar to Adafruit's CAP1188 ([#1602)](https://www.adafruit.com/product/1602) board, but without level-shifting, LEDs and pull-up hardware, and access to the WAKE and ALERT pins. The LDO voltage regulator chip has been left on-board for better power distribution.

#### I2C

The ADDR_COMM pins are routed to the host board where I2C comms mode is set and I2C addresses are set using programming resistors or routing to Vdd. The I2C pull-up resistors were moved to the host board to manage pull-down current better since four of five sensor modules could be in parallel, plus EEPROM or other components. The default address of Adafruit's CAP1188 board is available so that one of those can be connected to the QT Py MCU's Qwiic/STEMMA QT port and 5V.

### RESET

The RESET pins are routed to MCU on the host board and pulled high via a resistor network. The MCU pulls the RESET pins low when it's ready to configure the CAP1188.

### CAP1188 LED Drivers

#### The Default

The CAP1188 is designed to switch an LED cathode (-) to ground when a switch is active. LED anodes are pulled high with resistors. When a switch is not active, the N-channel MOSET is be off and the LED pin pulled high as well. Therefor the LED pin's state is inverted with respect to the switch state.

On startup the LEDs are switched off and the LED pins are high until the chip's configuration can be changed to invert the logic. If positive triggered solid state relays (SSRs) are connected to the LED pins, they will be switched on momentarily at startup.

### ---



It should be noted that there are no pull-up resistors for the CAP1188's LED drivers, so the LED drive mode will have to be changed from "open-drain" to "push-pull" mode to output a logic high. The output logic level can be inverted to provide a positive signal to trigger solid state relays requiring positive logic.

The CAP1188's default LED drive mode is open-drain such that when a switch is triggered, an n-channel MOSFET is turned-on switching the MOSFET drain and LED cathode to ground, illuminating the LED whos anode is pulled high.

The Cap Sensor's inputs (CS1-8) and LED outputs (LED1-8) are not routed through the host board. Outputs can be wired directly from the sensor board when needed - otherwise, switch status can be read from the status registers via I2C.

..<img src="../assets/sensor-mfg-top.png" alt="sensor top" style="zoom:50%;" /> <img src="../assets/sensor-mfg-bot.png" alt="sensor bottom" style="zoom:50%;" />

### Schematic

![schematic](../assets/schematic-sensor.png)

### Parts List

| Component |            Description             | Quan. |                         Part Number                          |
| :-------: | :--------------------------------: | :---: | :----------------------------------------------------------: |
|    U1     |      Cap Touch Sensor 24VQFN       |   1   | Microchip [CAP1188-1-CP-TR](https://www.digikey.com/short/jdhph3bp) |
|    U2     | LDO 3V3 Voltage Regulator SOT-23-5 |   1   | Microchip [MIC5225-3.3YM5-TR](https://www.digikey.com/short/h5855rbn) |

2x4 and 2x5 headers and housings may be useful. Through-hole pattern: 0.1" pitch, 0.040" holes.