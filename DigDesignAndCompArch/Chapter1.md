# Digital Logic Fundamentals

## 1.5: Logic Gates

Logic gates are simple digital circuits that take one or more binary inputs and produce a binary output.

### Common Logic Gates

- **XOR**: True if one of A or B is true
- **NAND**: False when A and B are both true, True all other cases
- **NOR**: True when both A and B are false

## 1.6: Beneath the Digital Abstraction

### Supply Voltage

- The lowest voltage in a system is 0V, also called **ground**
- The highest voltage comes from the power supply and is called **Vdd**

### Logic Levels

- The mapping of a continuous variable onto a discrete binary variable is done by defining logic levels
- The first gate is called the **driver** and the second the **receiver**
- The output of the driver is connected to the input of the receiver

### Noise Margins

The amount of noise that can be added to a worse-case output such that the signal can still be interpreted as a valid input.

**Formulas:**
- NM<sub>L</sub> = V<sub>IL</sub> - V<sub>OL</sub>
- NM<sub>H</sub> = V<sub>OH</sub> - V<sub>IH</sub>

## 1.7: CMOS Transistors

Transistors are electrically controlled switches that turn on and off when a voltage or current is applied to a control terminal.
