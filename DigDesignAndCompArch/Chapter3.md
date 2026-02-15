# Sequential Logic

## 3.1: Introduction

- The outputs of sequential logic depend on both the **current state** and **prior input values** → it has memory
- Sequential logic might explicitly remember certain previous inputs or might break prior inputs into smaller amounts of information called the **state of the system**
- The **state** of a digital sequential circuit is a set of bits called **state variables** that contain all the information about the past necessary to explain the future behavior of the circuit

## 3.2: Latches and Flip-Flops

- The fundamental building block of memory is called a **bistable element**, an element with two stable states
- An element of N stable states conveys log₂(N) bits of information, so a bistable element stores **one bit** of information
- Latches and flip-flops provide inputs to control the value of the state variables, which is much more practical than cross-coupled inverters as they have no input for the user to enter

### 3.2.1: SR Latch

Recall that a NOR gate produces FALSE when either input is true.

**Truth Table:**

| S | R | Q | ~Q |
|---|---|---|-----|
| 0 | 1 | 0 | 1 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 0 |
| 0 | 0 | Q<sub>prev</sub> | ~Q<sub>prev</sub> |

**Explaining Case IV (Memory):**
- If we assume that Q has a previous value Q<sub>prev</sub>
- If S = 0 and R = 0, whatever Q<sub>prev</sub> is will be the output for that case
- ~Q will be the complement, which is always true
- Therefore Q = Q<sub>prev</sub> and ~Q = ~Q<sub>prev</sub>
- **The circuit has memory**

**Explaining Case III (Invalid State):**
- Asserting (setting to 1) both S and R makes no sense because you are trying to set and reset simultaneously
- This makes the circuit confused, making both outputs 0

**Operation:**
- When **R** is asserted, the state is **reset to 0**
- When **S** is asserted, the state is **set to 1**
- When neither is asserted, the state **retains its old value**

### 3.2.2: D Latch

The D latch has two inputs:
- **D (Data input)**: controls what the next state should be
- **CLK (Clock)**: controls when the state should change

**Operation:**
- When CLK = 0, both S and R are false
- When CLK = 1, the latch is **transparent**: data at D flows through to Q as if the latch were just a buffer
- When CLK = 0, the latch is **opaque**: it blocks new data from flowing through to Q, and Q retains its old value

### 3.2.3: D Flip-Flop

- The D flip-flop can be built from **two back-to-back D latches** controlled by complementary clocks

**Operation:**
- When CLK = 0: the master latch is transparent and the slave latch is opaque. Whatever value was at D propagates through to N1
- When CLK = 1: the master goes opaque and the slave transparent. Whatever value was at D immediately before the clock rises from 0 to 1 gets copied to Q immediately after the clock rises

**Key Property:**
A D flip-flop copies D to Q on the **rising edge** of the clock, and remembers its state at all other times.

### 3.2.4: Register

An **N-bit register** is a bank of N flip-flops that share a common CLK input, so that all bits of the register are updated at the same time.
