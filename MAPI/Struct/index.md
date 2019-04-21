---
title: SlashWork
---
[<< Back](../)

# Struct
Games utilize data structures to store multiple variables into one place, all to serve a very specific purpose. It is crucial to modders that these data structures are also made available to them.

## Definition
Each struct is defined with three types: the struct itself, the wrapper, and the API abstraction class.

The struct is exposed only as a declaration and a public definition is not provided. It is defined as an empty struct inside of the implementation files, so that the actual version-specific structs can derive from it. The version-specific structs are defined only inside of the implementation files and must not ever be exposed to the public API.

The wrappers do most of the heavy-lifting to ensure that version-specific behavior is resolved. The empty constructor is deleted to prevent modders from mistakenly instantiating an empty wrapper instance. Instead, a single non-explicit constructor with a single parameter being a pointer to the struct instance is included. Getters and setters are provided to allow version-independent access and modification of each struct member's values. The const wrapper provides getters and wraps a const pointer to the struct. The (writable) wrapper provides setters, wraps a non-const pointer to the struct, and is a derived class from the const wrapper.

The API abstraction class is a standalone class meant to be used by modders to create instances of the struct, and it is a child class of the (writable) wrapper. It provides an explicit constructor that allows modders to construct an instance of the struct in a version-independent manner. The element is stored in a std::unique_ptr to auto-free memory when it falls out of scope. It does not provide a constructor that takes a pointer to an instance of the struct. However, it makes a call to the base class's constructor to ensure that the inherited functions work correctly. Additional functions may be added to streamline the usage of game functions.

The struct should be named similar to how it is used in the game code. The const wrapper's name is based on the struct's name, but with the suffix "\_ConstWrapper" to distinguish it from the original struct. The (writable) wrapper uses a similar monicker, but with the suffix "\_Wrapper". The API abstraction class is also based on the struct's name, but with the suffix "\_MAPI".
