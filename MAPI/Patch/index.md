---
title: SlashWork
---
[<<< Back](../)

# Patch
Custom game code is often used by modders to achieve changes not normally provided by the game engine. It is a must-have for mods that introduce significant changes.

## Patch and Restore
The patches are capable of overriding a region of memory in the game. If desired, the patches could also unpatch the game and restore the values that were originally at that region of memory.

## Patch Types
Three common patch types are included to simplify the process of patching a game. These patches are the Buffer Patch, the Branch Patch, and the NOP Patch.

The Buffer Patch is standard for most patch implementations, as it takes a sequence of raw bytes and overrides any position in game memory with these bytes. Typically, these patches are small and can be contained in the region of memory being modified.

The NOP Patch is a shorthand way to specify a Buffer Patch filled only with NOP instructions. This essentially replaces game code with instructions to do nothing.

The Branch Patch is used where game code needs to be replaced, but the new custom code is very large or advanced. A custom function is called where the patch occurs, which hands control of the game state to the modder. The are two branch types: call branch and jump branch. A call branch saves some state related to the instruction pointer to allow returning to the caller function. The jump branch does not save any state, which makes it useful for replacing entire functions. The size of the patch can also be specified, which will override the remaining bytes with NOP instructions.

## Design
The patches all use the same code for patching the game and restoring the old values. Therefore, it was not useful to separate the different patch types into different classes. Each patch is instead given static functions to initialize a patch with the proper values corresponding to the patch type. Patches also only accept one address and do not specify game version. This is to cut down on the amount of code needed for initialization and leaves the process of version checked patching to the modder. Most game versions also require patches that are unique for that patch and cannot be transferred between versions.
