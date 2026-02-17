# Question 1:

![Screenshot 2026-02-16 at 6 35 01 PM](https://github.com/user-attachments/assets/f81a5c3b-716e-4b62-8365-1cefd1238c47)

# Ring Oscillator Circuit

## Propagation Delay
Propagation delay is the time it takes for the output to change value after the input changes.

## Circuit Behavior
Let **X = 1** initially. Then:
- Y = 0
- Z = 1
- X = 0 (after one loop)

This is inconsistent, as after each loop the values are inverted. This circuit has no stable states, so it is said to be **unstable** or **astable**.

### Stability Rule
- **Odd number of inverters** → Unstable circuit
- **Even number of inverters** → Stable circuit

## Timing Analysis
An inverter outputs the opposite of its input, but it takes **1ns** for this to occur in our case.

### Timing Sequence
Let's set our first state **X = 1**:

1. X falls at **0ns** (inverter inverts 1 → 0)
2. Y = 0, which causes Y to rise at **1ns**
3. Z falls at **2ns**
4. X rises at **3ns**
5. Y falls at **4ns**
6. Process repeats...

### Oscillation Pattern
Each node oscillates between 0 and 1 with a **repetition time of 6ns**.

This means that if X rises at **(q)ns**, then it will rise again at **(q + 6)ns**.
