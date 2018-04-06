# Diablo II Pointers

Individual variables and functions of Diablo II may need to be accessed by individual modules for reading, writing, or execution.

This article is shared with Variables, Functions, and Stubs.

## History

See [here](../Version). Offsets to functions and variables had to be replaced entirely in order to port them to another version. Once ported, these changes made the pointers invalid for the old version.

Function declarations on standard compilers support the following x86 calling conventions. The first is the cdecl convention, where all of the function arguments are pushed onto the stack and cleanup is done by the calling function. The second is the stdcall convention, which does the same as cdecl, except that stack cleanup is done inside of the callee functions. The third is fastcall, which works similar to stdcall, except that the first two arguments are stored in the CPU registers ecx and edx. There are other calling conventions, such as thiscall and vectorcall, but functions are often not explicitly declared using these conventions.

The compiler for Diablo II, however, uses non-standard x86 calling conventions. The most common solution was to write a custom stub function just to execute these functions in their custom calling conventions. However, even with custom stubs being written, there was also the possibility of the calling convention or even the function signature changing across versions.

Another issue was that functions or variables could appear or disappear across versions. This could be problematic for version portability, as some mods or hacks could be heavily reliant on the behavior of certain functions or variables.

For developers, every pointer, whether for functions or variables, were kept in the same header file (.h) and heavily relied on macros to generate code. Compilation times dragged on if anything in that file was changed.

## The Problem

Diablo II pointers need to be version agnostic. Custom function declarations need to be version agnostic and shared redundant code for common custom calling conventions need to be shortened or removed. Compilation times need to be reduced. 

## The Solution

Separate functions and versions from each other. Eliminate stubs by abstracting the custom calling conventions. Functions and variables are declared using the header files, but the implementation is in the C++ code file (.cpp).

### Dependencies

- [Diablo II Offset](../Offset)
- [Diablo II Struct](../Struct)
- [Diablo II Version](../Version)
