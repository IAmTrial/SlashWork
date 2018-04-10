# Patch API

## History

See [here](../Diablo_II/Version/). Patches were defined in such a way that they do not check the game version before performing a patch. This meant that a patch for 1.13C could override game code in 1.13D, even though the addresses for the game code could be very different. In addition, patches on overlapping addresses overrode each other and was almost guaranteed to crash.

## The Problem

Game versions are not taken into account when patching the game. Overlapping patches crash without any warning.

## The Solution

Detect the game version and do not execute a patch for a game version that is not defined to work on that patch. For patches on overlapping addresses, provide a useful error message for mod makers to resolve these issues.

### Dependencies

- [Diablo II Offset](../Diablo_II/Offset/)
- [Diablo II Version](../Diablo_II/Version/)
