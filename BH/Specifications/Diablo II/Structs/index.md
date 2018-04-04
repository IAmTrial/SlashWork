# Diablo II Struct

Data variables in Diablo II are often the standard C data types. However, in some instances, object oriented design was used to group together multiple variables that when put together, fulfilled one particular job.

This article is shared with DataTables and Pakcets.

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
