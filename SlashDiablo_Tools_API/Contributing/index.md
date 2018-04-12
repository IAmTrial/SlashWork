# Contributing

SlashDiablo Tools is a community effort to provide various features to improve the quality of gameplay for Diablo II. The community is greatly welcoming of new changes that can work right out of the box. However, when it comes time to incorporate those changes, it is important to follow some preset standards.

## Language

The primary language of choice for almost every case is C++, using the latest standard revision. This is to ensure that old language constructs can be replaced by newer, more efficient ones. Also, when discussing C++, this is strictly talking about C++ and *not* C.

What we mean is that C language features are extremely discouraged and should be kept to a minimum as much as possible. Classic pointers, C-style arrays, and C character arrays are forbidden under almost all circumstances, because they can be safely replaced by smart pointers, std::array, and std::string/std::string_view, respectively. C-style arrays are permitted only when used as function arguments, but the size of that array must be passed in as the next argument. Declarations from rvalues, however, should use std::array wherever possible. Struct is acceptable if all members are declared const.

The only times where C code is acceptable are in two areas: the Windows API and the Diablo II API. 
	
For the Windows API, it is encouraged to use the “data types” that are defined by windows.h. This is permitted only because using these custom data types actually improves the readability of declared variables by specifying the intention of these variables. However, do not use these data types in the context of anything else. It is especially forbidden to mix windows.h data types with Diablo II API calls.

The Diablo II API requires C code because it is accessing very low levels of memory, where everything needs to be written correctly and aligned properly in order to work. In addition, custom assembly code is necessary for interfacing with Diablo II and the benefits of C++ are not appropriate there. However, this applies to code that directly interfaces with Diablo II. Wrapper code is not eligible for this exception.

Assembly code is written to match the architecture that Diablo II supports. Since x86 is the only architecture supported by Diablo II, there are some very important ground rules to address. x86 must be written using the Intel syntax, because that is the format used in many disassemblers. Writing assembly code in AT&T syntax is a guaranteed way to incorporate confusion and violate consistency guidelines.

## Compiler

The current primary compiler is MinGW-w64. This is not to be confused with MinGW (without w64), which is more than deficient in many language features, yet more than well-nourished with bugs. The second compiler of choice is the Clang LLVM Compiler. The third and final choice the Microsoft Visual C++ Compiler.

The choice of compiler is important, because the linker must be able to properly locate the correct memory addresses for API calls. In addition, various function attributes and ways to write assembly code are specific to compilers. Luckily, GCC and LLVM maintain a great amount of compatibility with each other, such that even the linkers are the same.

## Dependencies

The program is dependent on the following libraries:
- Shlwapi
- Version

Do not use “#pragma comment” in order to add libraries to the linker. This is non-portable and only specific to the Microsoft Visual C++ Compiler!

## Style Guidelines

Mostly based on [Google's C++ Style Guide](https://google.github.io/styleguide/cppguide.html).

However, these rules take precedence:
- All indentations are 4 spaces and no tabs.
- The line limit is 80 characters, including whitespace and newline.
  - This is to discourage unnecessarily nested block statements.
- Systems Hungarian notation (dwId, szName) for naming variables is disallowed.
- Apps Hunagarian is acceptable, depending on the circumstances (num_items, d_position_x).

However, the Diablo II API uses a slightly different convention:
- Variables and functions are capitalized for each first letter of a word, including the first word.

## Commit Messages

All commit messages, must follow the guidelines described [here](https://web.archive.org/web/20180314204455/https://chris.beams.io/posts/git-commit/).

## License

Since this program is licensed under the GNU Affero General Public License, version 3 or greater, all modifications and derivative works that link to the libraries must also be released under the same license (or a greater version). The best way to ensure that other people acknowledge the license is to place a boilerplate header at the top of the file, using the following template:

```C++
/**
 * <one line to give the program's name and a brief idea of what it does.>
 * Copyright (C) <year> <name of author>
 *
 * This file is part of <program name>.
 *
 *  This program is free software: you can redistribute it and/or modify
 *  it under the terms of the GNU Affero General Public License as published
 *  by the Free Software Foundation, either version 3 of the License, or
 *  (at your option) any later version.
 *
 *  This program is distributed in the hope that it will be useful,
 *  but WITHOUT ANY WARRANTY; without even the implied warranty of
 *  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 *  GNU Affero General Public License for more details.
 *
 *  You should have received a copy of the GNU Affero General Public License
 *  along with this program.  If not, see <http://www.gnu.org/licenses/>.
 */
```
Any file that does not contain this header at the very top will have one tacked on automatically.

In addition, a file named COPYING containing the full text of the AGPL v3+ must be placed at the top level of the project directory and must be distributed along with binaries.
