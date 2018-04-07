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

Separate functions and versions from each other. Eliminate stubs by abstracting the custom calling conventions. Functions and variables are declared using the header files, but the implementation is in the C++ file (.cpp).

### Dependencies

- [Diablo II Offset](../Offset)
- [Diablo II Struct](../Struct)
- [Diablo II Version](../Version)

## Implementation

### Eliminated Implementation(s)

Defining Diablo II variables using static initialization can lead to reading in uninitialized values. A static function that attempts to read the value of a statically initialized Diablo II variable may potentially read an undefined value. Thus, Diablo II variables shall not be implemented with statically initialization.

### Current Implementation

Offset is used to define Diablo II variables in a version agnostic way. Since variables are not as complex as functions and its signature cannot be easily invalidated across versions, Offset is acceptable for this purpose. The variable is declared in the header file, while its value is set in the C++ file.

The next section requires an external library type, [constexpr unordered map](https://github.com/benjibc/constexpr_hash_map).

Instead of defining the variables using static initialization, for the reason stated above, the variables are initialized when they are first needed. To achieve this, a wrapper class utilizing generics keeps track of all of the initialization values. The variables are declared constexpr and the wrapper class constructor is also declared constexpr. This guarantees construction on compile time and solves runtime static initialization. This is done in the header file.

When the wrapper class is first dereferenced, then the correct offset for the current GameVersion is determined. This overloads the operators [all operators for dereferencing](http://en.cppreference.com/w/cpp/language/operator_member_access) and [conversion](http://en.cppreference.com/w/cpp/language/cast_operator) to its contained pointer type. Conversion is permitted to be implicit. This is done in the C++ file.

The version agnostic function is declared in the header file. There may be multiple variants of this globally declared function, using Ex classes, but there is always at least one declaration of the function that uses the exact same function arguments as the function it references. If a struct pointer is an argument, then use the base struct type. These header functions should be the only Diablo II API functions accessible by mod makers and hackers.

The implementation details are in the C++ file and are described below.

The standard procedure for each function is to first declare the function pointers in a constexpr unordered map.

Next, declare all of the version specific functions and all required stubs. Version specific functions are directly tied to the actual functions called by Diablo II. They are named using the original, non-Ex function’s name, but with the version identifier appended after an underscore (e.g. D2Client_Foo_1_13D). Stubs use the same scheme, but with an addition “_stub” appended to its identifier (e.g. D2Client_Foo_1_13D_stub). If any version of a function shares same function signature with other versions, then attach a comment indicating this and use the identifier with the lowest value (e.g. use 1.10 instead of 1.13d).

Next, implement the header file function using a switch-case to check GameVersion and call the correct version specific function. Additional features may include conversions or retrieval of values for the Ex class.

Next, implement the version specific functions, which retrieve the correct function address using the offset found in the constexpr unordered map. If required, call the stub function, with the target function being the first argument.

Finally, implement the individual version specific functions. These functions are implemented using assembly in order to properly set up the register states for calling the desired function. The function first pops the return address off of the stack into a register. Next, the xchg instruction is used to swap values of that register with the first argument. Next, the register state and stack is set up to match the target function call using the pop and xchg instructions. When the state is fully set up, the function is called.

To reduce writing redundant assembly code, there are macros defined just to pop and xchg into one register. There are as many macros as there are registers (minus the instruction pointer register and stack pointer register).
