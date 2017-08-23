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

### JIT
When the JVM detects that a piece of code is executed very frequently, it does the Just In Time execution. This means that JVM converts that bytecode into optimized **machine code**. This is also called dynamic compilation.
