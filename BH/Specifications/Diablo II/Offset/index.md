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
