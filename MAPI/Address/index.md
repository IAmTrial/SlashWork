---
title: SlashWork
---
# Address
In order to patch or identify data or functions, there needs to be a way to specify a location in game memory. Game addresses serve this purpose.

## Base Address
To determine the location, the dynamically linked library needs to be specified. Any library that is associated with the inner workings of the game can be specified using their enum value instead of their path. All other libraries can be specified using their path. Base address cannot be specified via hardcoding due to the possibility of the library base addresses changing.

## Address Types
Addresses are specified in one of three ways: offset, ordinal, and decorated name.

An offset is very simple, because it adds an offset to the base address of the specified library.

An ordinal utilizes the ordinal value found in the function export table of the specified library.

A decorated name utilizes the name of the function found in the function export table of the specified library.

## Design
The addresses are constructed using one of the static functions designated for each address type. This is because a game address only maintains the address after performing the required initialization steps.
