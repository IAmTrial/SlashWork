# Diablo II Pointers

Individual variables and functions of Diablo II may need to be accessed by individual modules for reading, writing, or execution.

This article is shared with Variables, Functions, and Stubs.

## History

See [here](../Version). Offsets to functions and variables had to be replaced entirely in order to port them to another version. Once ported, these changes made the pointers invalid for the old version.

Function declarations on standard compilers support the following x86 calling conventions. The first is the cdecl convention, where all of the function arguments are pushed onto the stack and cleanup is done by the calling function. The second is the stdcall convention, which does the same as cdecl, except that stack cleanup is done inside of the callee functions. The third is fastcall, which works similar to stdcall, except that the first two arguments are stored in the CPU registers ecx and edx. There are other calling conventions, such as thiscall and vectorcall, but functions are often not explicitly declared using these conventions.

The compiler for Diablo II, however, uses non-standard x86 calling conventions. The most common solution was to write a custom stub function just to execute these functions in their custom calling conventions. However, even with custom stubs being written, there was also the possibility of the calling convention or even the function signature changing across versions.

Another issue was that functions or variables could appear or disappear across versions. This could be problematic for version portability, as some mods or hacks could be heavily reliant on the behavior of certain functions or variables.

For developers, every pointer, whether for functions or variables, were kept in the same header file and heavily relied on macros to generate code. Compilation times dragged on if anything in that file was changed.

## The Problem

Diablo II pointers need to be version agnostic. Custom function declarations need to be version agnostic and shared redundant code for common custom calling conventions need to be shortened or removed. Compilation times need to be reduced. 

## The Solution

Separate functions and versions from each other. Eliminate stubs by abstracting the custom calling conventions. Functions and variables are declared using the header files, but the implementation is in the C++ file.

### Dependencies

- [Diablo II Offset](../Offset)
- [Diablo II Structs](../Structs)
- [Diablo II Version](../Version)

## Implementation

### Eliminated Implementation(s)

Defining Diablo II variables using static initialization can lead to reading in uninitialized values. A static function that attempts to read the value of a statically initialized Diablo II variable may potentially read an undefined value. Thus, Diablo II variables shall not be implemented with statically initialization.

Defining a wrapper class to encapsulate a variable also fails, because construction of the type is still prone to static order initialization issues. Originally, the idea was to use constexpr types, with the assumption that the variables would be initialized at compile time instead. This assumption turned out to be faulty, leading to an abandonment of this strategy.

### Current Implementation

Offset is used to define Diablo II variables in a version agnostic way. Since variables are not as complex as functions and its signature cannot be easily invalidated across versions, Offset is acceptable for this purpose. The variable is declared in the header file, while its value is set in the C++ file.

A namespace is declared for each dynamic-link library (.dll). In addition, a set of files, namely the header and C++ files, is used specifically for declaring functions. Another set is specifically for declaring variables. Variables are kept in a sub-namespace called Vars, while the functions are in a sub-namespace called Funcs.

Variables are declared in the header file as functions that return a pointer to the intended Diablo II variable. This is somewhat confusing, but it is guaranteed to solve static initialization order issues. Inside the C++ file, the function implementation contains an unordered map containing all of the pointers is declared and initialized. Next, a static variable containing the target pointer is declared and initialized using the GameVersion and the unordered map of pointers. Afterwards, the target pointer to the Diablo II variable is returned.

The version agnostic function is declared in the header file. There may be multiple variants of this globally declared function, using Ex classes, but there is always at least one declaration of the function that uses the exact same function arguments as the function it references. If a struct pointer is an argument, then use the base struct type. These header functions should be the only Diablo II API functions accessible by mod makers and hackers.

The implementation details are in the C++ file and are described below.

The standard procedure for each function is to first declare all of the version specific functions and all required stubs. Version specific functions are directly tied to the actual functions called by Diablo II. They are named using the original, non-Ex function’s name, but with the version identifier appended after an underscore (e.g. Foo_1_13D). Stubs use the same scheme, but with an addition “_stub” appended to its identifier (e.g. Foo_1_13D_stub). If any version of a function shares same function signature with other versions, then attach a comment indicating this and use the identifier with the lowest value (e.g. use 1.10 instead of 1.13d).

Next, implement the header file function using a switch-case to check GameVersion and call the correct version specific function. Additional features may include conversions or retrieval of values for the Ex class.

Next, implement the version specific functions. The function declares a static unordered map containing relevant function pointers. The function retrieves the correct function address using the offset found in the unordered map. If required, call the stub function, with the target function being the first argument.

Finally, implement the individual version specific functions. These functions are implemented using assembly in order to properly set up the register states for calling the desired function. The function first pops the return address off of the stack into a register. Next, the xchg instruction is used to swap values of that register with the first argument. Next, the register state and stack is set up to match the target function call using the pop and xchg instructions. When the state is fully set up, the function is called.

To reduce writing redundant assembly code, there are macros defined just to pop and xchg into one register. There are as many macros as there are registers (minus the instruction pointer register and stack pointer register).
