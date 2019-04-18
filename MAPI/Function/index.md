---
title: SlashWork
---
# Function
A game function is any block of code that can be called for repeated results. They must be properly identified before modders can safely use them in their own mods.

## Similarities to Game Data
Much of the article on [Data](../Data/) applies here. However, functions are a somewhat more specialized form of data in that it can be invoked in order to control what the game is doing at some given moment.

## Calling Convention
The standard calling convention on most compilers is the __cdecl calling convention. It is also notable as being the only calling convention that can be used in variadic functions. In addition, the naming convention for such functions with the C naming scheme is simply the function name with an underscore prefix.

Game developers may not always choose __cdecl as the calling convention for their own functions. In some cases, __stdcall, __ecxcall, and __thiscall are chosen instead. Some developers also attempt to thwart modding attempts by changing the calling convention of their functions. Others even unintentionally make modding attempts more difficult by using a compiler that optimizes a function to utilize a custom calling convention.

For this reason, the API seeks to normalize all function calling conventions to __cdecl so that modders would have only one calling convention to concern themselves with. All parameters are then properly passed on to the actual function, taking the actual calling convention into consideration.
