# Patch API

## History

See [here](../Diablo_II/Version/). Patches were defined in such a way that they do not check the game version before performing a patch. This meant that a patch for 1.13C could override game code in 1.13D, even though the addresses for the game code could be very different. In addition, patches on overlapping addresses overrode each other and was almost guaranteed to crash.

## The Problem

Patches should detect the game version and should not execute for a game version that is not defined to work on that patch. In addition, patches on overlapping addresses should provide a useful error message for mod makers to resolve these issues.
