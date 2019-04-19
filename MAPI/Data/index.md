---
title: SlashWork
---
[<<< Back](../)

# Data
Game data are scattered throughout the game's memory as variables and requires proper identification in order to allow modders to properly access and change their values.

## Static Initialization Order Fiasco
In order to properly adjust the game data's pointer during run-time, global variables cannot be used. This is because of the infamous [Static Initialization Order Fiasco](https://isocpp.org/wiki/faq/ctors#static-init-order) that results in retrieving uninitialized data. The solution is to use a function that initializes the pointer variable on the first time the function is called.

## Getters and Setters
Getters and setters are used to control access and altering of the game data values. A single function returning the pointer would not suffice, as the size of the pointed to data can change at the whim of the game developers. An improper size can result in overriding undesirable areas of memory or writing too little and getting undesirable game behavior. Getters and setters provide an additional level of protection to prevent developers from thwarting mods by simply changing the data size.

## Naming
Data should be named using the following order of succession:
1. Ingame Name
2. Game Manual Name
3. Name Based on the Observed Effects of the Data
4. Debugging Name or Internal Name in the Code
5. Common Name (i.e. the name the modding community gives it)

This order was chosen to prevent inconsistent and confusing names. All too often, the game developers decide on a name that is only understood by the development team. Some modding communities also decide on a name that is highly inaccurate or does not properly express what the variable does in the game. Thus, a name that is close to what is explicitly defined in the game text reduces the likelihood of choosing arbitrary names.

For boolean-type variables, the variable should start with the "Is" or "Has" verb to indicate a binary status. All getters and setters should be prefixed with Get and Set, respectively.

## Separation of Declaration and Definition
The declaration and definition of the variables should be kept in different places. The user should not need to know the underlying abstraction of the API. It is also beneficial to the compile time if the headers and the source were to be kept separate.

Macros are a common way to quickly declare and define variables quickly, but they incur the cost of ramping up the compile time. In addition, they do not have the flexibility of individually identifying variables with data sizes that change between versions.

## Address Definition
In order to keep the file size small, the definition of addresses are not hardcoded into the API. Instead, addresses are read in from a table, with the name of the table matching the game version being played. The address table maintains the path to the associated dynamically linked library, the name of the address, the address locating type, and the address locating value.
