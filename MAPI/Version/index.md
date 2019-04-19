---
title: SlashWork
---
[<< Back](../)

# Version
Some mods are heavily dependent on the game version being used. Sometimes, the mod can be made usable in more than one game version. It would be desirable if the mod was compatible across all versions of the game.

## Enumerations
Each game version is enumerated to reduce the likelihood that a modder might enter the incorrect version information for their mods.

## Functions
The API always exports functions to determine the version string using a game version enumeration value, and a function to retrieve the currently running game version's enumeration value.

## Detection
Version detection is dependent on the game. One way to check for the game version is to rely on the file version reported by the game executable. When that fails, checking a region of memory for specific values may yield unique results.
