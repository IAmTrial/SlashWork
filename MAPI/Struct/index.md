---
title: SlashWork
---
# Struct
Games utilize data structures to store multiple variables into one place, all to serve a very specific purpose. It is crucial to modders that these data structures are also made available to them.

## Definition
Each struct is defined with two types: the struct itself and the wrapper.

The struct is exposed only as a declaration and a public definition is not provided. It is defined as an empty struct inside of the implementation files, so that the actual version-specific structs can derive from it. The version-specific structs are defined only inside of the implementation files and must not ever be exposed to the public API.

The wrapper does most of the heavy-lifting to ensure that version-specific behavior is ironed out. The empty constructor is deleted to prevent modders from mistakenly instantiating an empty wrapper instance. Instead, a single non-explicit constructor taking a pointer to the struct instance is included. The wrapper provides a static Create function to allow modders to construct an instance of the struct in a version-independent manner. A static Destroy function is also provided to free resources. Getters and setters are provided to allow version-independent access and modification of each struct member's values. Additional functions may be added to streamline the usage of game functions.

The struct should be named similar to how it is used in the game code. The wrapper's name is based on the struct's name, but with the suffix "_MAPI" to distinguish it from the original struct.
