# Gateway Node

Gateway Node + 16DI Base Chassis)
Role: Expert Hardware Engineer & Senior PCB Layout Designer.
Project Goal: Create Schematics and PCB Layouts for a modular 19-inch 1U industrial automation system (Access Control and Telemetry). The system consists of two mating PCBs:

Base Chassis (16 DI Module) — acts as an IO board and backplane.

Compute Node (Gateway Cassette) — a pluggable brain module sliding into the base chassis.
Interconnect: Blind-mating via a rear DB25 connector.

Generate the schematic netlist definitions, component selections, and PCB layout constraints for both boards according to the following specifications:

BOARD 1: Compute Node (Gateway Cassette)
Form-factor: Compact pluggable card (Cassette). Front panel faces outside the rack; rear edge connects to the Base Chassis.

1. Core & Memory:

MCU: K1948VK018 (MIK32 Amur, RISC-V). Clock: 32 MHz Industrial Crystal.

Memory Bus (I2C): AT24C512 EEPROM (for network settings) and DS3231M RTC (with CR1220 battery holder).

Memory Bus (SPI): W25Q32 (32-Mbit Flash) for local event logging ("Blackbox"). Separate CS pin.

Debug: 6-pin JTAG header (TCK, TMS, TDI, TDO, GND, RESET) for factory programming.

2. Networking (Hardware VLAN):

Switch: Microchip KSZ8863 (3-port managed switch). Managed via I2C/SMI from MCU.

MAC/PHY: WIZnet W5500 (TCP/IP controller) connected to MCU via SPI.

Routing: KSZ8863 Port 3 connects directly to W5500 PHY via capacitive coupling.

Front Panel: KSZ8863 Ports 1 and 2 route to 2x RJ-45 MagJacks (with built-in magnetics and LEDs) on the front edge of the PCB.

3. Console & Debug:

USB: Type-C 16-pin connector on the front edge. CC1/CC2 with 5.1k pull-downs.

Bridge: CH340N (USB-to-UART) connected to MCU hardware UART.

4. Rear Interconnect (DB25 Male):

Connector Type: D-Sub 25-pin Male (DB25), mounted on the rear edge for blind mating.

Pinout Mapping:

Pins 1, 2, 3: VCC_24V_IN (Main power received from Base Chassis).

Pins 4, 5, 6: GND.

Pin 7: RS-485_A (Local bus to Base Chassis).

Pin 8: RS-485_B (Local bus to Base Chassis).

Pin 9: 1-Wire (DATA for DS18B20 external temp sensor).

Pins 10-25: Reserved (NC / Future use).

Power Subsystem on Gateway: Receive 24V from DB25 -> Step-down DC-DC (to 5V) -> LDO (to 3.3V). Include 1F 5.5V Supercapacitor (Last Gasp) with charging resistor and SS34 discharge diode on the 5V line.
