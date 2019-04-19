---
title: SlashWork
---
[<<< Back](../)

# Function
A game function is any block of code that can be called for repeated results. They must be properly identified before modders can safely use them in their own mods.

## Similarities to Game Data
Much of the article on [Data](../Data/) applies here. However, functions are a somewhat more specialized form of data in that it can be invoked in order to control what the game is doing at some given moment.

## Calling Convention
The standard calling convention on most compilers is the \_\_cdecl calling convention. It is also notable as being the only calling convention that can be used in variadic functions. In addition, the naming convention for such functions with the C naming scheme is simply the function name with an underscore prefix.

Game developers may not always choose \_\_cdecl as the calling convention for their own functions. In some cases, \_\_stdcall, \_\_ecxcall, and \_\_thiscall are chosen instead. Some developers unintentionally make modding attempts more difficult by using a compiler that optimizes functions to utilize a custom calling convention. Others even attempt to thwart modding attempts by changing the calling convention of their functions between versions.

For this reason, the API seeks to normalize all function calling conventions to \_\_cdecl so that modders would have only one calling convention to concern themselves with. All parameters are then properly passed on to the actual function, taking the actual calling convention into consideration.
