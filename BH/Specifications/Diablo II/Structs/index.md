# Diablo II Struct

Data variables in Diablo II are often the standard C data types. However, in some instances, object oriented design was used to group together multiple variables that fulfilled one particular job.

This article is shared with DataTables and Packets.

## History

Structs in BH share a somewhat similar story with [Diablo II Version](../Version). However, there are some key differences in how and why they are different. Compilers often implement structs by making them appear no different from simply defining their individual member variables in order. If a struct never changes this ordering, then its implementation in machine code won’t be different either.

This means that unlike offsets, which are different due to the compiler and linker, structs across multiple versions are different *on purpose* because the Diablo II developers modified the struct’s variable order. The developers don’t suffer in the same way that mod makers do, because they always have access to that particular variable using its identifier. Meanwhile, mod makers have to reverse engineer a struct to know the exact order a struct is implemented.

It is also believed that these structs are actually originally classes, since C++ makes a very small distinction between the two. However, thanks to the compiler and its zero-cost abstractions, it is very difficult to know which functions were originally associated with which class.

## The Problem

Individual structs that have differences across versions must be redefined.

## The Solution

Define a common data structure that provides a proper one-to-one equivalent to a Diablo II struct, independent of game version.

### Dependencies

- [Diablo II Version](../Version)

## Implementation

### Eliminated Implementations

Declaring a base struct and providing virtual functions through that struct has been tried before, with disastrous effects. The reason being that the compiler adds an extra variable if even one function is declared in the struct. Not only do data member locations have to be correct, but the struct size must also be correct. Thus, getting alignment with the corresponding Diablo II struct is impossible to achieve using this strategy.

### Current Implementation

One could create a class that interfaces with the structs across different versions. The individual structs from each version can be declared such that they are separate from each other, but linked together through inheritance. This allows them to be referenced in a polymorphic manner using a smart pointer. When this class is initialized, the correct struct is initialized at run time, using the detected GameVersion.

To further abstract this concept, each individual struct is declared in their own separate header file. The declaration of the base struct is available in this header file. This base struct is always empty and uses the name of structs that are commonly defined by the Diablo II modding and hacking communities (e.g. struct D2Example). The version-specific structs are kept in the .cpp file and are given the same name as the base struct, but with the version identifier appended after an underscore (e.g. struct D2Example_113d). These version-specific implementations are child structs of the base struct. As for the class, it is declared in the header file and uses the same name as the base struct, but with “Ex” being the prefix to the type name (e.g. class ExD2Example). This class is referred to as the Ex class and was inspired by the naming scheme used by the developers of D2Ex2.

The following are guarantees provided by the Ex class. All data members of the struct are accessible regardless of version differences. Each class provides functions that enable access or modification to all common data members. There is always one function that will return a raw C pointer to the underlying struct as a raw C pointer to the base struct. This allows users of the API to modify structs without having to worry about implementation details. In addition, the Diablo II functions can be further abstracted such that they take the “Ex” class and handle the raw C pointer from there.

The Ex classes, depending on how their underlying struct behaves, can also contain additional functions that call Diablo II functions. This encourages object oriented design and reduces the need to know which Diablo II function to call for a specific struct.

