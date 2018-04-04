# SlashDiablo Tools Project

## History

Around 2011, the very first iteration of the BH maphack for Diablo II was written by McGod of the BlizzHackers forum, hence why the hack is called BH. The source code and injector were made available to the public, with the injector licensed under the GNU Affero GPL, version 3.

In 2011, Deadlock39 modified the BH hack (original post [here](https://web.archive.org/web/20180403231144/https://www.reddit.com/r/slashdiablo/comments/m1o5a/update_v012_for_bh_branch/)), customizing it for the SlashDiablo community. One year later, Underbent obtained a copy of the source code, made his changes for v1.1.3, and released those changes to the public (original post [here](https://web.archive.org/web/20180403231445/https://www.reddit.com/r/slashdiablo/comments/1286jz/bh_maphack_v013/)). The source code has been made available [here](https://github.com/underbent/slashdiablo-maphack).

Over the years, additional contributors, including slippittydo, decker87, slayergod13, lolisquad, planqi, and Mir Drualga, added their own custom changes on top of the original hack, incorporating new features and fixing bug present in previous versions. This continued community development has allowed the maphack to become what it is today.

## Perceived Issues with BH

While BH is great at providing plenty of quality-of-life features that improve the rate at which players can progress through Diablo II, it is not without its faults.

The hack was built using the Microsoft Visual C++ Compiler, which is proprietary and Windows-centric. This affects Linux users who may want to compile the source code, but are not left with much of a choice when there are language extensions and constructs that only exist on that particular compiler. In addition, the compiler is developed at its own pace and often does not support new C++ language features as quickly as other C++ compilers.

The original BH was published in 2011. In addition, in the second half of 2011, C++11 had been approved, revamping many of the ways people write C++ code. It is evident in the BH source code that many of the features have been written using now obsolete, difficult-to-grasp, and dangerous patterns that make maintenance a nightmare.

## Purpose of SlashDiablo Tools

SlashDiablo Tools was created to incorporate all the various features provided by BH and others, while maintaining high standards for code. In order to maintain this standard, various areas are reconstructed with these newer, better practices in order to provide examples of what kind of expectations we have for each tool.

The original BH keeps all of its code closely knit together. This type of pattern was acceptable for the original hack, simply on the basis that it “just works.” It was also constructed in this way so as to avoid being detected by Warden. However, BH appears to take approaches that are highly difficult to follow and could have been made more efficient in some areas. The issue of being detected by Warden is not an issue for SlashDiablo folks, since maphack is perfectly allowed.

The approach that is taken is to maintain the original BH as the API to Diablo II. All modules are built as separate projects, dependent on the API. The API provides wrappers to enable access to any variable or function that is needed by a module. It is also version agnostic, meaning that the same code will “magically” work across the different versions of Diablo II. BH also provides the structures needed to construct custom patches.

## License

SlashDiablo Tools is licensed under the GNU Affero General Public License, version 3 or greater. But it was not always that way.

As noted, the injector for BH has been licensed under the GNU Affero General Public License, version 3 or greater. Unfortunately, McGod has only added these notices to the injector and not the maphack itself. This ambiguity allows anyone to take the source code of the maphack, make modifications to improve it, and distribute the binaries while never publishing the source. This goes against the spirit of what this project has been founded upon.

To discourage this behavior, SlashDiablo Tools has been licensed under the GNU Affero General Public License, version 3 or greater. This effectively prevents anyone from directly using the source code and redistributing binaries without providing the source code. Not only that, but if any of the source code or binaries are used for game servers, then that source must also be made available, as required by the *Affero* version of the GPL. If a violation of the license has been spotted, please contact the SlashDiablo or Diablo2Resurgence team on the appropriate Discord channels.

## Legal

Diablo® II<br>
©2000 Blizzard Entertainment, Inc. All rights reserved. Diablo and Blizzard Entertainment are trademarks or registered trademarks of Blizzard Entertainment, Inc. in the U.S. and/or other countries.

Diablo® II: Lord of Destruction®<br>
©2001 Blizzard Entertainment, Inc. All rights reserved. Diablo, Lord of Destruction and Blizzard Entertainment are trademarks or registered trademarks of Blizzard Entertainment, Inc. in the U.S. and/or other countries.
