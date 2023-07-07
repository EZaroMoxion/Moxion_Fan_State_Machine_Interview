# Introduction
MoxionPower has a lot of cooling fans in our system. Your task today is to implement a controller for one of our fans. We value correct and well tested code at MoxionPower. We hope that you would feel comfortable using your solution in our production codebase.

# System Overview
## Sensor Power Enable
This GPIO enable line controls the power supply for our temperature sensor. It must be enabled for our temperature sensor to provide valid measurements. The sensor power supply takes 5 seconds to stabilize after a rising edge on the enable line. Control of this signal code is provided via the `GpioOutputInterface& sensor_power_enable` object.

## Temperature Sensor
An Analog Devices AD22100 temperature sensor is used by our system to sense the voltage of our inverter. The datasheet for this sensor is included. We are powering the sensor using a 5V supply voltage. Your code will be passed a `VoltageSensorInterface& temp_sensor_raw` reference that can be used to get the raw value of the voltage input.

## Fan Control Enable
First there is a GPIO enable line that controls a relay powering the fans. When this output is low, power is cut to the fans and they will stop, drawing no system power. When this output is high, power will be supplied to the fans. Control of this signal code is provided via the `GpioOutputInterface& fan_relay_enable` object.

## Fan Control PWM Duty Cycle
When power is provided to the fans they will start turning. The speed the fans turn at is determined by the duty cycle of the provided PWM signal. Control of this signal code is provided via the `PwmOutputInterface& fan_output_raw` object.

# Requirements
1. The controller shall enable sensor power prior to using temperature measurements for fan control and wait 5 seconds before considering a temperature measurement valid.
2. The controller shall enable the fan control relay when the temperature measurement is above 60C.
3. The controller shall disable the fan control relay when the temperature measurement is below 50C.
4. The controller shall implement the following table to determine output fan duty cycle to command at a given temperature.

| Temperature (t) | Output = f(t)        |
|-----------------|----------------------|
| 60-99           | = (60 - t) / 2       |
| 100-129         | = (100 - t) * 2 + 20 |
| 130-140         | = 90                 |
| > 140           | = 100                |

5. The controller shall implement a power saving mode where sensor power is disabled if the temperature measurement is less than 20C for more than 1 minute.
6. The controller shall periodically renable sensor power to measure the temperature every 10 minutes if it has been disabled when in this power saving mode.
7. The controller shall exit power saving mode when the measured temperature exceeds 25C during a periodic sample period.

# Instructions

1. Using the extended set of requirements above, design a state machine that will fulfill the above requirements
2. Draw a visual representation of this state machine
3. Talk through your design process, and rationale behind your design

# Criteria

1. Evaluation will be done on the following aspects of your state machine:
- completeness
- simplicity
- well-defined states
- clear transitions

2. Explanation of your process. The goal of this exercise is to understand your design process, and how you think about implementation of requirements. The best way to relay this information is to talk through your process and ask any questions as you go along.


  