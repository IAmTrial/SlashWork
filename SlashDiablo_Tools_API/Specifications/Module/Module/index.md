# Module

Every change to the game's behavior is defined into a module. A module is customized specifically to make that one change and can be ignored if not desirable.

## Implementation

### Current Implementation

The current implementation of module is very much the same as the one used in the past.

A module is the middle layer for implementing custom behavior for Diablo II. It contains functions that are called upon when certain game events occur. In addition, the module is often used to store patches and apply them on certain events.

The old design originally had the individual module add itself to the module manager. This implementation is not ideal, since it can introduce a level of indirection and confusion. Instead, the developer is responsible for inserting the module into the module manager by calling the BH::addModule function.
