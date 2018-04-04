# Contributing

SlashDiablo Tools is a community effort to provide various features to improve the quality of gameplay for Diablo II. The community is greatly welcoming of new changes that can work right out of the box. However, when it comes time to incorporate those changes, it is important to follow some preset standards.

## Language

The primary language of choice for almost every case is C++, using the latest standard revision. This is to ensure that old language constructs can be replaced by newer, more efficient ones. Also, when discussing C++, this is strictly talking about C++ and *not* C.

What we mean is that C language features are extremely discouraged and should be kept to a minimum as much as possible. Classic pointers, C-style arrays, and C character arrays are forbidden under almost all circumstances, because they can be safely replaced by smart pointers, std::array, and std::string/std::string_view, respectively. Struct is acceptable if all members are made const.

The only time where C code is acceptable are in two areas: the Windows API and Diablo II API. 
	
For the Windows API, it is encouraged to use the “data types” that are defined by windows.h. This is permitted only because using these custom data types actually improves the readability of the variable by specifying the intention of these variables. However, do not use these data types in the context of anything else. It is especially forbidden to mix windows.h data types with Diablo II API calls.

The Diablo II API requires C code because it is accessing very low levels of memory, where everything needs to be written correctly and aligned properly in order to work. In addition, custom assembly code is necessary for interfacing with Diablo II and the benefits of C++ are not appropriate there. However, this applies to code that directly interfaces with Diablo II. Wrapper code is not eligible for this exception.

Assembly code is written to match the architecture that the Diablo II supports. Since x86 is the only architecture supported by Diablo II, there are some very important ground rules to address. x86 must be written using the Intel syntax, because that is the format used in many disasseblers. Writing it in AT&T syntax is a guaranteed way to incorporate confusion and violate consistency guidelines.
