# Real-Time C++ - Chapter 1 Notes

*Efficient Object-Oriented and Template Microcontroller Programming - 2nd Edition*

---

## 1.3 Class Types

Class types enable object-oriented programming in C++ because they group data together with functions operating on them in a self-contained identity.

### Initialization Rules

- **Constant member variables** of a class must be initialized in the constructor initialization list
- **Non-constant member variables** should be initialized in the constructor initialization list

### Scope Resolution Operator (`::`)

When members are defined outside of a class definition, the scope resolution operator (`::`) is used to resolve the class name from the names of members in the implementation file.

> **Note:** This is typically used when you have separate `.hpp` and `.cpp` files. When you declare your functions in the `.hpp` file, you need the scope resolution operator when implementing them in the `.cpp` file.

---

## 1.4 Members

### Example: LED Toggle Function

The `led` class has a member function `toggle()`:

```cpp
void toggle() const 
{ 
    // Toggle the LED. 
    *reinterpret_cast<volatile uint8_t*>(port) ^= bval; 
}
```

**Function Purpose:**
- Responsible for toggling the LED from off to on and vice versa
- Toggling port pin is carried out with bit manipulation through direct memory access

### Const Member Functions

A constant member function is not usually intended to alter the state of any class member variables.

**Exception:** `const` can change member variables that have the `mutable` keyword.
- `mutable` means capable of being changed

### Access Controls

In C++, members of a class type have one of three access controls:

| Access Control | Description |
|---------------|-------------|
| **public** | Constitute the class's user interface; can be accessed by any part of the program |
| **private** | Can only be accessed by the class itself and its friends |
| **protected** | Useful for code re-use via inheritance in class hierarchies |

---

## 1.5 Objects and Instances

- **Object:** A class type that represents an actual thing, concept, or group/collection thereof that can be manipulated as a cohesive entity
- **Instance:** An occurrence of a class type

---

## 1.6 #Include

Files such as library files or user-defined header files can be included in another file with the `#include` syntax.

---

## 1.7 C++ Standard Library

The standard library is a vast collection of types, functions, and classes that is an essential part of the C++ language.

### Standard Template Library (STL)

The standard library contains an extensive set of generic containers and algorithms called the **Standard Template Library (STL)**.

---

## 1.9 The main() Subroutine

### Return Type

- In C++, the return type of `main()` is a plain integer, a signed `int`
- When we write `int` in C++, we mean `signed int` (the default for integral types if left blank is signed)

### Subroutines

A **subroutine** is another name for any block of code that is to perform a specific task.

### Return Statements

All subroutines other than `main()` returning any type other than `void` must supply explicit return code.

---

## 1.10 Low-Level Register Access

Microcontroller programming in C++ requires low-level register access.

### Example: C++ Register Access

```cpp
void toggle() const 
{ 
    // Toggle the LED.
    // C++ register access
    *reinterpret_cast<volatile uint8_t*>(port) ^= bval; 
}
```

### reinterpret_cast

The templated cast operator `reinterpret_cast` is one of the four specialized cast operators available in C++. It is designed for casting integral types to pointers and back.

---

## 1.11 Compile Time Constant

### constexpr Keyword

A generalized constant expression, denoted with the keyword `constexpr`, is guaranteed to be a compile-time constant.

**Benefits:**
- Using `constexpr` is generally better than `#define` because generalized constant expressions have clearly defined type information

### Constant Folding

If variables are compile-time constants, the compiler can initialize the variables without using the stack or intermediate CPU registers. This efficient kind of optimization is called **constant folding**.

---

## Source

Based on notes from: *Real-Time C++: Efficient Object-Oriented and Template Microcontroller Programming - 2nd Edition*

**Original PDF:** [nibmehub.com](https://nibmehub.com/opac-service/pdf/read/Real-Time%20C++_%20efficient%20Object-Oriented%20and%20template%20microcontroller%20programming-%202nd%20Edition.pdf)