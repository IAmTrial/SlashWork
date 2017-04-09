# Diablo 2 Widescreen Hack

## Quick Description of the Why
Diablo II was released in 2001 to only support 640x480. One year later, the expansion, Lord of Destruction, added 800x600 resolution to the game. Those resolutions use 4:3 aspect ratio. And after that, Blizzard never added more any new resolutions.

Oh yeah, and modern computer displays now handle resolutions better than that. And Sven wrote the Glide to OpenGL wrapper, which opens up Diablo II's graphical features to people that don't own 3dfx GPUs. A few hackers also released D2MultiRes, which allows Diablo II to be played at any resolution, but its official support only existed for version 1.12.

Most people don't have monitors with a 4:3 aspect ratio. Nowadays, the 16:9 aspect ratio has become popular. Some even own monitors with the 21:9 aspect ratio! It would be nice if Diablo II could natively support the 16:9 aspect ratio without stretching the display or putting the ugly black bars to the sides.

## More Recent/Relevant History Leading Up to the Hack
On April 23, 2014 Reddit user /u/TravHatesMe released a hack at /r/SlashDiablo that allows Diablo II to be played at 1300x700 resolution, but required version 1.13**d**, while most players on /r/SlashDiablo were playing using version 1.13**c**. The fact that the hack had massive amounts of graphical bugs didn't help either. Two years passed, and I, /u/IAmTrial took the liberty of adding several fixes to the code that improve the overall graphics display, fixed item movement macros, and backported the hack to version 1.13c. After getting approval from /u/kaesekuchenx3 aka PhyRaX, I released my 'fixed' hack to /r/SlashDiablo.

A few folks at /r/SlashResurgence also wanted to get a port of the hack to their modded server, since the hack would simply crash if run on their client. In addition, several users, requested for a working Glide mode (and PlugY mode, credits to /u/updawg for requesting both). I asked /u/Fohg, the head moderator for permission...and this new hack was born.

## How It was Achieved
The first thing that was done was deciding what resolution to use. /u/Fohg aka Fog of /r/Diablo2Resurgence wanted to add a natively supported 1066x600 mode for his server. This was chosen specifically because it wouldn't expand the game's field of vision to an imbalanced amount. In addition, the height wouldn't change, but the width would, so that it would be true 16:9 aspect ratio.

Diablo II has several hardcoded values that determine which resolution mode it should display. In addition, several values were also hardcoded, which determine the positioning of certain UI elements. I determined that the best way to implement 1066x600 mode was by replacing 640x480 mode in the resolution menu.

By opening up Cheat Engine, I needed to locate the instruction pointers for instructions that are associated with setting 640x480 resolution values. The easiest way to do this was by changing the resolution and finding out which values were altered.

There were three files that must be modified in order to enable HD mode. These files are D2gfx.dll, D2gdi.dll, and D2client.dll.

### Modifying D2gfx.dll 
The instruction pointer 0x7FE8 to 0x7FF3 of D2gfx.dll looks like this:
```markup
C7 00 80020000        mov [eax], 00000280         ; move the value 640 into the value storing the screen width
C7 01 E0010000        mov [eax], 000001E0         ; move the value 480 into the value storing the screen height
```
This instruction resizes the Diablo II game window when it detects a resolution change. Leaving these instrcutions unaltered will not crash the game, but it causes the game to crunch the elements together into a resolution that it cannot fit. This affects the way the game perceives the mouse position and UI elements. The mouse cannot move past the 640th x-position and the 480th y-position of the game window. It also creates terrible looking black lines.

So the solution was this:
```markup
C7 00 2A040000        mov [eax], 0000042A         ; move the value 1066 into the value storing the screen width
C7 01 58020000        mov [eax], 00000258         ; move the value 600 into the value storing the screen height
```


0x6D55 and 0x706B of D2gdi.dll
0xDCD0 of D2glide.dll

TODO
0xC3A11 of D2client.dll for menu tab positioning
0x10E29 of D2client.dll


