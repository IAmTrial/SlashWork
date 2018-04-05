# Diablo II Offset

Game offsets are utilized in order to define where the individual variables and functions of Diablo II are located.

## History

See [here](../Version).

## The Problem

The input of offsets does not account for different versions of Diablo II. Porting BH from one version to another requires replacing every single offset.

## The Solution

Determine the correct address to use for each variable or function.

### Dependencies

- [Diablo II Version](../Version)

## Implementation

### Eliminated Implementation(s)

The different offsets were originally stored into one struct. However, this was faulty because this encouraged implicit declaration of offsets, without knowing which version each offset applies to. Not only that, but each and every offset definition was directly affected by the order of declaration. When a new version was added, this struct also had to be updated. Adding a new version entry at the center a struct would result in changing all declaration of offsets to be updated to realign the order.

### Current Implementation

Dynamically determine which offset to use, based on the version number. Add this offset to the associated module address in order to get the full target address.

The enum GameVersion and an unordered map are used to locate the correct offset in constant time. The benefit of using GameVersion is that it is an enum class, which means that all key declarations are explicit. An unordered map, with GameVersion as the key and long long int as the key, provides the constant time lookup that the struct originally provided. In addition, the unordered map could have empty entries and still perform its job correctly. New versions can be added to GameVersion API and backwards compatibility is preserved.

The primary reason for using a signed value for offsets is primarily due to ordinal values. While positive values represent offsets, negative values represent ordinal values.

One of the member function in Offset calls Version’s functions in order to get the current GameVersion value. Next, the member function get the offset mapped to that GameVersion from the unordered map of offsets. Afterwards, it returns this value.

The enum DllFiles contains the list of dynamic-link libraries (.dll) used by Diablo II. This enum is declared enum class to enforce explicitness.

One of the static member functions in Offset keeps a static unordered map, where DllFiles is the key and the module handles are the value.  This unordered map starts out empty. The entries are filled out when a module is needed, using GetModuleHandle and LoadLibrary. However, a special case applies for 1.14 and above. If those versions are detected, then the module handle to obtain is redirected to Game.exe, for all .dll files that have been removed in those versions. For detection, an unordered set is used to find if a .dll is part of that group in constant time. The base address of the module or Game.exe is returned at the end of this function.

One of the member functions in Offset calls upon its own functions to retrieve the appropriate offset and the base address of the desired module. If the offset is positive, it adds these two values and returns the sum. If the offset is negative, then GetProcAddress is used to retrieve the address of the function.
