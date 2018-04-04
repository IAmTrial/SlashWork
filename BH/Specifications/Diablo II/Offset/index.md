# Diablo II Offset

Game offsets are utilized in order to define where the individual variables and functions of Diablo II are located.

## History

See [here](../Version).

## The Problem

The input of offsets does not account for different versions of Diablo II. Porting BH from one version to another requires replacing every single offset.

## The Solution

Dynamically determine which offset to use, based on the version number. Add this offset to the associated module address in order to get the full target address.

### Dependencies

- [Diablo II Version](../Version)

## Methodology

### Current Active Method

Grab the version number using the “Diablo II Version” and retrieve the correct address for that version.

## Implementation

## Eliminated Approach

The different offsets were originally stored into one struct. However, this was faulty because this encouraged implicit declaration of offsets, without knowing which version each offset applies to. Not only that, but each and every offset definition was directly affected by the order of declaration. When a new version was added, this struct also had to be updated. Adding a new version entry at the center a struct would result in changing all declaration of offsets to be updated to realign the order.
