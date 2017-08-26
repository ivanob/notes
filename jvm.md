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
1. Load the class into memory using the classloader and generates the **class object**. Class objects live in the method area.
2. VERIFICATION: Checks if the class is well formed or not (follow the rules of Java) as a security check.
3. PREPARATION: Allocate space for the static variables (and initialize them with default values)
4. RESOLUTION: Load referenced classes: Load the classes that are referenced in Hello.class. To do that, they follow the same process (dynamic linking).
5. INITIALIZATION: Initialize variables: Set the real value to the static variables previously allocated.

NOTES:
- Before loading and initializing a class, we load first all his **superclasses**
- If the type is an **interface**, it will be loaded but not initialized. From Java8, as interfaces can have static methods, it is also loaded in case it contains one static method or a field that is accessed via static method.

## CLASSLOADER
[Pic class loader]

- The first step that the classloader does, is check on the heap if the class object already exists.
- If it doesnt exists it tries to find the .class file following the parent delegation model.
- If its found, it creates the class object. Otherwise, a ClassNotFoundException is thrown.

Parent delegation model: When the classloader needs to find the file it looks in several repositories starting from the most reliable first. If it doesn't find the file in the first one it checks in the next one, and so on. The last repositories are not trustworthy at all and can contain malicious classes. These are the repositories:
1. Bootstrap class loader: load all the core classes
2. Extension C.L: load classes from the extension API
3. Application C.L: load classes from the developer
4. User-Defined C.L: custom classloaders written in Java by other developers

When is a **Class** loaded for first time?:
- New instance of that class is created.
- One of its static methods is accessed.
- One of its static fields is accessed, except for compile-time constants because these values are replaced by compiler during compilation.
- If one of its subclasses is loaded.
- If the class is loaded from command-line.
- The class is accessed via reflection.

When is a **Interface** loaded for first time?: Same as before, but obviously the first case (new instance) is impossible in instances.

The output of the classloading proccess is the class object, which is used by JVM every time it needs to create a new instance. It just contains meta-info of the **Class, Interface, Primitive, Void or Array**.

## LINKING (verification, preparation, resolution)

### Verification
- Check that the bytecode of the loaded class is safe and well formed.
- Follows the Java rules: final classes can not be subclassed, no illegal method overloading...

### Preparation
- It allocate space for static fields and initialize them with default values. If there is not space available it throws an OutOfMemoryError.
- If the class is not only loaded but also initialized, then it also initializes the instance fields (attributes) of the class after the static ones.

### Resolution
It is the process of replacing symbolic references with direct references to memory.
When the classloader generates a class object, it keeps all the references from our class to external classes as **symbolic references** or logic references. They are stored inside the **class object** in a place called **constant pool**. Constant pool also contains String literals and compile-time constants of that class.
- The resoluter goes through all the constant pool and it resolves each symbolic reference. To do that, if the class referenced is not in heap already it loads it starting a new lifetime of that type.
- Once its loaded, the resolutor replaces the symbolic link with the direct reference. This direct reference is a reference to memory in the heap, where the real object referenced lives.

Java uses the technic of **dynamic linking**: The resolution of links is done in runtime. The advantage of this is that programs are easier to update, as every time I load a class into memory, it will reload all the references of it. there is no need to recompile. We can update our program just replacing the updated class in the classpath with the new version. There are 2 approaches to load references: eager loading and lazy loading.

On the other hand, **static linking** occurs during compilation. The code of the referenced file is copied into the final executable during the compilation time.

While resolving references, the resolutor checks that the current class has permission to the referenced class. It also checks that the fields really exists in the referenced class and their types are correct.

## INITIALIZATION
Initialize the static fields with the values the user defined, replacing the default ones. Interfaces are only initialized if they have static methods (> Java8) or one of its variables (non compile-time) are accessed.





# resources
- http://www.artima.com/insidejvm/ed2/
- https://www.pearson.com/us/higher-education/program/Hunt-Java-Performance/PGM182574.html#
- https://www.codeproject.com/Articles/24029/Home-Made-Java-Virtual-Machine
- https://stackoverflow.com/questions/13624462/where-does-class-object-reference-variable-get-stored-in-java-in-heap-or-stac
