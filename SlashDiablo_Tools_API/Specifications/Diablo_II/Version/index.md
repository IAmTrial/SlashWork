# Diablo II Version

Diablo II was released in 2000, and Lord of Destruction was released in 2001. There is no doubt that the game has gone through multiple revisions in order to fix bugs, revamp the code base, and introduce new features.

## History

The original BH written very specifically for Diablo II 1.13C. All pointers to Diablo II functions and data have been statically declared. When version 1.13D was released, all the pointers for the old version were invalid for this new version. Thus, all pointers had to be replaced with the new ones in order to work with Diablo II 1.13D. The result of this is two code bases that have their differences in the pointer storage file. End users who aren’t able to distinguish between 1.13C and 1.13D (ingame shows “v 1.13”) end up confused as to which file to choose. Then version 1.14 was released, and the cycle repeats yet again…

## The Problem

It is rather cumbersome having to maintain the entire function table, especially when trying to keep the code bases up to date. Part of the problem is that the game does not provide some way to detect the version given ingame. Everything is compiled to work just for one specific version.

## The Solution

Provide an interface to detect the game version in a consistent manner.

## Implementation

### Eliminated Implementation(s)

Starting Diablo II, version 1.14, almost all dynamic-linked library (.dll) files have been eliminated. Thus, any attempt to read from those files is impractical.

All other .dll files are useless, because they do not contain Diablo II’s version information. Diablo II.exe is also useless because of this.

Reading the version information from Game.exe directly using ReadProcessMemory could work, except that it is impractical because it needs to try every known address where the file version is known. This could be a problem when a game version is not defined.

Storing a hash of Game.exe and dynamically generating a hash for that file is impractical, because Game.exe could have as little as one bit changed, and the whole hash value changes. Also, the file used in hashing has to be located.

### Current Implementation

The version information is read from Game.exe using Windows API functions. This enables consistent and dynamic retrieval of the game’s version string.

The Windows API functions used to detect versions require linking to the Version library.

Since version information can be and should be accessed globally, all calls to access the variables should be done with global functions, to prevent modification. In order to not pollute the global namespace, all functions are incorporated under a single namespace. These functions should also be accessed by modmakers creating works based on SlashDiablo Tools.

Hardcoded version strings is unintuitive for contributors. An enum that represents version information, named GameVersion, is highly preferable because it offers explicitness. However, to prevent accidental implicit conversions from integers, the enum is declared using enum class or enum struct.

Lookup of the version string should take constant time. Thus, an unordered map is used to store a mapping of the version string to the corresponding GameVersion value. Pairs can be added to the version string map when needed. This interface is provided through a function.
