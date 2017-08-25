# Introduction
One of the biggest problems faced by compiled languages was the necessity of recompile a program every time you want to run it in a different architecture. In response of this, **interpreted languages** were created, although their performance would never be as fast as a **compiled language**.

Engineers in SunMicrosystems decided to mix both approaches and they created Java: It is a compiled language (it is fast), but it is also **platform independent** due to it runs over a **virtual machine**.

A virtual machine is like a real architecture, but it does not exists physically. JVM has it own instruction set (the bytecode). When we compile a program in Java, we obtain a .class file where the source code has been translated into bytecode. JVM gets this file and it is able to execute it.

JVM is described by the following resources:
- Abstract JVM Specification: document that describes JVM features and how it should work.
- Java Language Specification (JLS): Defines the syntax and semantics of Java language.
- JVM implementation: it is a reference implementation of the JVM.

As a very general idea, what it happens when we run a program is:
1. An instance of the JVM implementation is loaded in memory. This is called **runtime instance**.
2. The .class file is also loaded in memory.
3. The JVM interprets the bytecode from the file.

This is a diagram of the JVM:

### JIT
When the JVM detects that a piece of code is executed very frequently, it does the Just In Time execution. This means that JVM converts that bytecode into optimized **machine code**. This is also called dynamic compilation.

---
**Note:** From now on, all the descriptions will happen in Runtime. The input of the JVM is the compiled program.

#  Lifetime of a type
We call type to any class or interface. Lifetime of a type refers to the process of loading a class file into the JVM until it is ready for execution. Lifetime of a type is divided in 3 phases: **Classloader, Linking and Initialization**. So I will describe what happens when the user writes "class Hello.class" in the command line.

[INSERT LIFETIME PIC]

## Overview
First of all, the JVM checks if the class is already loaded in memory. If not, it needs to load it so it follows this process:
1. Load the class into memory using the classloader and generates the **class object**.
2. VERIFICATION: Checks if the class is well formed or not (follow the rules of Java) as a security check.
3. PREPARATION: Allocate space for the static variables (and initialize them with default values)
4. RESOLUTION: Load referenced classes: Load the classes that are referenced in Hello.class. To do that, they follow the same process (dynamic linking).
5. INITIALIZATION: Initialize variables: Set the real value to the static variables previously allocated.

NOTES:
- Before loading and initializing a class, we load first all his **superclasses**
- If the type is an **interface**, it will be loaded but not initialized. From Java8, as interfaces can have static methods, it is also loaded in case it contains one static method or a field that is accessed via static method.


# resources
- http://www.artima.com/insidejvm/ed2/
- https://www.pearson.com/us/higher-education/program/Hunt-Java-Performance/PGM182574.html#
- https://www.codeproject.com/Articles/24029/Home-Made-Java-Virtual-Machine
-
