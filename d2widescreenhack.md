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
The instructions for 0x7FE8 to 0x7FF3 of D2gfx.dll look like this:
```markup
C7 00 80020000          mov [eax], 00000280         ; move the value 640 into the value storing the screen width
C7 01 E0010000          mov [eax], 000001E0         ; move the value 480 into the value storing the screen height
```
This instruction resizes the Diablo II game window when it detects a resolution change. Leaving these instrcutions unaltered will not crash the game, but it causes the game to crunch the elements together into a resolution that it cannot fit. This affects the way the game perceives the mouse position and UI elements. The mouse cannot move past the 640th x-position and the 480th y-position of the game window. It also creates terrible looking black lines.

Solution:
```markup
C7 00 2A040000          mov [eax], 0000042A         ; move the value 1066 into the value storing the screen width
C7 01 58020000          mov [eax], 00000258         ; move the value 600 into the value storing the screen height
```
### Modifying D2gdi.dll
There are two modifications that need to be made for this file. The first modification goes to the instructions from 0x6D55 to 0x6D5E of D2gdi.dll.
```markup
BE 80020000         mov [eax], 00000280         ; move the value 640 into the value storing the game's width render
BE E0010000         mov [eax], 000001E0         ; move the value 480 into the value storing the game's height render
```
These instructions set the values used to determine how Diablo II should render the game. If unaltered, the screen will stretch the 640x480 resolution to cover the extra space added to the display window. Oh yeah, and the game crashes shortly afterward.

Solution:
```markup
BE 28040000         mov [eax], 00000428         ; move the value 1064 into the value storing the game's width render
BE 58020000         mov [eax], 00000258         ; move the value 600 into the value storing the game's height render
```
Note that the width that was entered was **1064**. For some reason, entering anything higher than 1064 will cause the width rendering to overflow onto the left side of the screen. Despite this minor quirk, this fix forces Diablo II to actually render the full 1066x600 resolution instead of some stretched hybrid.

The next instruction to modify is at the instruction from 0x706B to 0x7074 of D2gdi.dll.
```markup
C7 05 9CCA876F 80020000         mov [eax], 00000280         ; move the value 640 into the value storing the x-position where the game should stop rendering
```
This instruction tells Diablo II where to stop rendering entities such as wall or pillars. If left unmodified, anything past the x-position at 640 will not be rendered, or at least correctly. The game won't crash, at least.

Solution:
```markup
C7 05 9CCA876F 2A040000         mov [eax], 0000042A         ; move the value 1066 into the value storing the x-position where the game should stop rendering
```
### Modifying D2client.dll


TODO
0xC3A11 of D2client.dll for menu tab positioning
0x10E29 of D2client.dll
0xDCD0 of D2glide.dll

