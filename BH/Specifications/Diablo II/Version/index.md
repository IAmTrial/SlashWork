# Diablo II Version

Diablo II has been released in 2000 and its expansion, Lord of Destruction, has been released in 2001. There is no doubt that the game has gone through multiple revisions in order to fix bugs, revamp the code base, and introduce new features.

## History

The original BH written very specifically for Diablo II 1.13c. All pointers to Diablo II functions and data have been statically declared. When version 1.13d was released, all the pointers for the old version were invalid for this new version. Thus, all pointers had to be replaced with the new ones in order to work with Diablo II 1.13d. The result of this is two code bases that have their differences in the pointer storage file. End users who aren’t able to distinguish between 1.13c and 1.13d (ingame shows “v 1.13”) end up confused as to which file to choose. Then version 1.14 was released, and the cycle repeats yet again…

## The Problem

It is rather cumbersome having to maintain the entire function table, especially when trying to keep the code bases up to date. Part of the problem is that the game does not provide some way to detect the version given ingame. Everything is compiled to work just for one specific version.

## The Solution

Provide a way to detect the game version in a consistent manner.
