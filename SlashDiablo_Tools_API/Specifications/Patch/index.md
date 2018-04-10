# Patch API

Modmakers and hackers often need to apply custom patch code in order to change game behavior.

## History

See [here](../Diablo_II/Version/). Patches were defined in such a way that they do not check the game version before performing a patch. This meant that a patch for 1.13C could override game code in 1.13D, even though the addresses for the game code could be very different. In addition, patches on overlapping addresses overrode each other and was almost guaranteed to crash.

## The Problem

Game versions are not taken into account when patching the game. Overlapping patches crash without any warning.

## The Solution

Detect the game version and do not execute a patch for a game version that is not defined to work on that patch. For patches on overlapping addresses, provide a useful error message for mod makers to resolve these issues.

### Dependencies

- [Diablo II Offset](../Diablo_II/Offset/)
- [Diablo II Version](../Diablo_II/Version/)

## Implementation

### Current Implementation

All patches inherit from the abstract class BasePatch. BasePatch provides pure virtual functions to install a patch and restore the game code before a patch. It also provides a function to store the gam code before being overridden. The BasePatch is responsible for storing the Offset, which contains all the information needed to locate an address. It is also responsible for storing the patch size, since it uses the patch size to determine how many bytes of game code to save.

The InterceptionPatch is a child class of BasePatch. Its main purpose is to replace game code with a call or jump to an interception function. For this, it stores the address of the target interception function and the assembly opcode used. On the x86 architecture, the patch size must be greater or equal to 5 bytes. These first 5 bytes are used to replace game code with the interception function call or jump. After 5 bytes, the game code is replaced with no op instructions.

The BufferPatch is a child class of BasePatch. Its purpose is to copy code from a buffer and replace the game code with the values stored in that buffer. A buffer and its size are provided in the constructor, where a new buffer data member is constructed. This data member stores a copy of all the data in the buffer, up to the amount specified by the size. When a patch is applied, all the values stored in the buffer replaces the game code at the specified offset.
