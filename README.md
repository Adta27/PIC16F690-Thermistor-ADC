# PIC16F690 Thermistor ADC Temperature Display

This project is a temperature sensing circuit built using a PIC16F690 microcontroller and assembly language. It uses a thermistor as an analog input, reads the temperature-related voltage through the PIC16F690 ADC, and displays the temperature level using 8 LEDs.

The goal of this project was to learn how analog sensors, ADC conversion, microcontroller programming, and PCB design work together in an embedded system.

## What It Does

- Reads temperature changes using a thermistor voltage divider circuit
- Uses the PIC16F690 ADC on RB4 / AN10 / physical pin 13
- Converts the analog thermistor voltage into a digital ADC value
- Displays the temperature level using LEDs connected to PORTC
- Uses assembly language to configure the ADC, read values, and control outputs
- Includes schematic and PCB design files

## How It Works

The thermistor changes resistance when the temperature changes. This changes the voltage going into the PIC16F690 ADC pin.

The PIC16F690 reads this analog voltage using its ADC module. The program stores the ADC result and compares it to different threshold values. Depending on the ADC value, different LEDs turn on to show the temperature range.

In this circuit, a higher ADC value means colder temperature, and a lower ADC value means hotter temperature.

## LED Output

The LEDs are connected to PORTC:

| PIC Pin | Port Pin | LED |
|---|---|---|
| Pin 9 | RC7 | LED 4 Yellow |
| Pin 8 | RC6 | LED 5 Green |
| Pin 5 | RC5 | LED 8 Blue |
| Pin 6 | RC4 | LED 7 Blue |
| Pin 7 | RC3 | LED 6 Green |
| Pin 14 | RC2 | LED 3 Yellow |
| Pin 15 | RC1 | LED 2 Red |
| Pin 16 | RC0 | LED 1 Red |

## Code

The full assembly source code is in:

```text
code/thermistor_adc.asm
