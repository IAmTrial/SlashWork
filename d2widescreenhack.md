# Diablo 2 Widescreen Hack

## Quick Description of the Why
Diablo II was released in 2001 to only support 640x480. One year later, the expansion, Lord of Destruction, added 800x600 resolution to the game. These resolutions use 4:3 aspect ratio. And after that, Blizzard never added more any new resolutions.

Oh yeah, and modern computer monitors now handle resolutions better than that. And Sven wrote the Glide to OpenGL wrapper, which opens up Diablo II's graphical features to people who don't own 3dfx GPUs. A few hackers also released D2MultiRes, which allows Diablo II to be played at any resolution, but it only officially supported version 1.12.

Most people don't use monitors with the 4:3 aspect ratio. Nowadays, the 16:9 aspect ratio has become more widespread. Some even own monitors with the 21:9 aspect ratio! It would be nice if Diablo II could natively support the 16:9 aspect ratio without stretching the display or putting black bars to the sides.

## More Recent/Relevant History Leading Up to the Hack
On April 23, 2014 Reddit user /u/TravHatesMe released a hack at /r/SlashDiablo that allows Diablo II to be played at 1300x700 resolution, but required version 1.13**d**. Most players on /r/SlashDiablo were playing the game on version 1.13**c**. The fact that the hack had massive amounts of graphical bugs didn't help either. Two years passed, and I, /u/IAmTrial took the liberty of adding several fixes to the code that improve the overall graphics display, fixed item movement macros, and backported the hack to version 1.13c. After getting approval from /u/kaesekuchenx3 aka PhyRaX, I released the fixed hack to /r/SlashDiablo.

A few folks at /r/SlashResurgence also wanted to get a port of the hack to their modded server, since the hack would crash with the mod. In addition, several users requested for a working Glide mode (and PlugY mode, credits to /u/updawg for requesting both). I asked /u/Fohg, the head moderator for permission...and this new hack was born.

## How It was Achieved
The first thing that was done was deciding what resolution to use. /u/Fohg aka Fog of /r/Diablo2Resurgence wanted to add a natively supported 1066x600 mode for his server. This was chosen specifically because it wouldn't expand the game's field of vision to an imbalanced amount. In addition, the height wouldn't change, but the width would, so that it would have true 16:9 aspect ratio support.

Diablo II has several hardcoded values that determine which resolution mode it should display and where to position certain UI elements. I determined that the best way to implement 1066x600 mode was by replacing 640x480 mode in the options menu.

I needed to locate the instruction pointers for instructions that are associated with setting 640x480 resolution values. The easiest way to do this was for me to use Cheat Engine. I would change the resolution and finding out which values were altered.

There were three files that must be modified in order to enable HD mode. These files are D2gfx.dll, D2gdi.dll, and D2client.dll.

### Modifying D2gfx.dll 
The instructions from 0x7FE8 to 0x7FF3 of D2gfx.dll look like this:
```markup
C7 00 80020000          mov [eax], 00000280         ; move the value 640 into the value storing the screen width
C7 01 E0010000          mov [ecx], 000001E0         ; move the value 480 into the value storing the screen height
```
These instructions resize the Diablo II game window when it detects a resolution change. Leaving these instructions unaltered will not crash the game, but it causes the game to crunch the elements together into a resolution that it cannot fit. This affects the way the game perceives the mouse position and UI elements. The mouse cannot move past the 640th x-position and the 480th y-position of the game window. It also creates terrible looking black lines.

#### Solution:
```markup
C7 00 2A040000          mov [eax], 0000042A         ; move the value 1066 into the value storing the screen width
C7 01 58020000          mov [ecx], 00000258         ; move the value 600 into the value storing the screen height
```
### Modifying D2gdi.dll, Part 1
There are two modifications that need to be made for this file. The first modification goes to the instructions from 0x6D55 to 0x6D5E of D2gdi.dll.
```markup
BE 80020000         mov esi, 00000280         ; move the value 640 into the value storing the game's width render
BE E0010000         mov edx, 000001E0         ; move the value 480 into the value storing the game's height render
```
These instructions set the values used to determine how Diablo II should render the game. If unaltered, the screen will stretch the 640x480 resolution to cover the extra space added to the display window. Oh yeah, and the game crashes shortly afterward.

#### Solution:
```markup
BE 28040000         mov esi, 00000428         ; move the value 1064 into the value storing the game's width render
BE 58020000         mov edx, 00000258         ; move the value 600 into the value storing the game's height render
```
Note that the width that was entered was **1064**. For some reason, entering anything higher than 1064 will cause the width rendering to overflow onto the left side of the screen. Despite this minor quirk, this fix forces Diablo II to actually render the full 1066x600 resolution instead of some stretched hybrid.
### Modifying D2gdi.dll, Part 2
The next instruction to modify is at the instruction from 0x706B to 0x7074 of D2gdi.dll.
```markup
C7 05 9CCA876F 80020000         mov [D2gdi.dll+0xCA9C], 00000280          ; move the value 640 into the value storing the x-position where the game should stop rendering
```
This instruction sets up where Diablo II should stop rendering entities such as walls or pillars. If left unmodified, anything past the x-position at 640 will not be rendered, or at least correctly. The game won't crash, at least.

#### Solution:
```markup
C7 05 9CCA876F 2A040000         mov [D2gdi.dll+0xCA9C], 0000042A          ; move the value 1066 into the value storing the x-position where the game should stop rendering
```
### Modifying D2client.dll, Part 1
Now that all of the rendering problems are ironed out, the main issue to tackle is the game logic, held together by D2client.dll. Here are the instructions from 0x10E29 to 0x10E3C:
```markup
B8 80020000                     mov eax, 00000280                             ; move the value 640 into the eax register
A3 48BCB86F                     mov [D2client.dll+0xDBC48], eax               ; move the value of eax into the value storing the game's logic for width
C7 05 4CBCB86F E0010000         mov [D2client.dll+0xDBC4C], 000001E0          ; move the value 480 into the value storing the game's logic for height
```
These instructions set up Diablo II's field of vision, character and map positioning, and where to position certain UI elements. If left unmodified, the game will render the game as if it were in 640x480 mode and place the game on the upper left side of the window.

#### Solution:
```markup
B8 2A040000                     mov eax, 0000042A                             ; move the value 1066 into the eax register
A3 48BCB86F                     mov [D2client.dll+0xDBC48], eax               ; move the value of eax into the value storing the game's logic for width, Note that this is unmodified
C7 05 4CBCB86F 58020000         mov [D2client.dll+0xDBC4C], 00000258          ; move the value 600 into the value storing the game's logic for height
```
### Modifying D2client.dll, Part 2
The game is ready to play, except that the UI panels display is somewhat wacky. Quest buttons, waypoint buttons, and mercenary stats do not align correctly. Here are the instructions directly responsible for 640x480 mode panel placement, which starts at 0xC3A11 and ends at 0xC3A1C.
```markup
89 1D A0B9BC6F          mov [D2client.dll+0x11B9A0], ebx          ; move the value of ebx (which is 0) into the value that stores the x-offset positioning of panels
89 1D A4B9BC6F          mov [D2client.dll+0x11B9A4], ebx          ; move the value of ebx (which is 0) into the value that stores the y-offset positioning of panels
```
Now there's a problem. Moving a register into a memory location requires less bytes than moving a DWORD constant, which means that I need a total of 8 extra bytes if I want to add HD mode. While I could probably replace the entire set of instructions with a call to another function, and NOP the rest of the instructions, I went for a more simplistic route. Here is a much wider view of the instructions I could modify, spanning 0xC39F9 to 0xC3A1C:
```markup
75 16                           jne D2client.dll+0xC3A11                      ; jump to 640x480 code if not 800x600 mode
C7 05 A0B9BC6F 50000000         mov [D2client.dll+0x11B9A0], 00000050         ; move the value 80 into the value that stores the x-offset positioning of panels
C7 05 A4B9BC6F C4FFFFFF         mov [D2client.dll+0x11B9A4], FFFFFFC4         ; move the value -60 into the value that stores the y-offset positioning of panels
EB 0C                           jmp D2client.dll+0xC3A1D                      ; jump to code after 640x480 code
89 1D A0B9BC6F                  mov [D2client.dll+0x11B9A0], ebx              ; move the value of ebx (which is 0) into the value that stores the x-offset positioning of panels
89 1D A4B9BC6F                  mov [D2client.dll+0x11B9A4], ebx              ; move the value of ebx (which is 0) into the value that stores the y-offset positioning of panels
```
Here, we have a view of 800x600's code. What is convenient is that the height remains the same when switching from 800x600 mode to 1066x600 mode and vice versa. By reordering the code, I can reuse the code that sets up the y-offset positioning for 800x600 mode, which will then open up 6 bytes since the 640x480 mode y-offset code can be safely overridden. In addition, the total number of bytes that need to be written have been reduced from 8 bytes to 4 bytes. Now I can make my modifications.

#### Solution:
```markup
C7 05 A4B9BC6F C4FFFFFF         mov [D2client.dll+0x11B9A4], FFFFFFC4         ; move the value -60 into the value that stores the y-offset positioning of panels
75 0C                           jne D2client.dll+0xC3A11                      ; jump to 1066x600 code if not 800x600 mode
C7 05 A0B9BC6F 50000000         mov [D2client.dll+0x11B9A0], 00000050         ; move the value 80 into the value that stores the x-offset positioning of panels
EB 0C                           jmp D2client.dll+0xC3A1D                      ; jump to code after 1066x600 code
89 1D A0B9BC6F D5000000         mov [D2client.dll+0x11B9A0], 000000D5         ; move the value 213 into the value that stores the x-offset positioning of panels
90                              nop                                           ; extra leftover byte, do nothing
90                              nop                                           ; extra leftover byte, do nothing
```
Now the game is ready to play at 1066x600 resolution...assuming that you run only windowed DirectDraw mode and that you have the proper TXT files for properly aligning the inventory elements. Oh yeah, and BH's item moving macros don't work anymore.


TODO
0xDCD0 of D2glide.dll

