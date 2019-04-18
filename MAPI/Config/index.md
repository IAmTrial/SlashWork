---
title: SlashWork
---
# Configuration

A user configuration should permit control of mod features, at the discretion of the mod authors. It is also just as important to standardize the configuration format to prevent inconsistencies between mod authors.

## Format
The selected format for general user configuration is the JSON format, followed to the exact specification and without extensions or omissions. This format was chosen for its ability to remain organized, provide meaningful keys, and its type system.

## Developer-Only Information
Only one specific character for the start of a key should be reserved by the developers to indicate developer-only configuration information, and that is the exclamation mark (e.g. !) character. This character was chosen due to its position on the [ASCII table](http://www.asciitable.com/), with a value of 33. This was chosen instead the space character (value of 32) due to its visibility to end-users as well. A properly written configuration should store the developer information in a single key prefixed by three exclamation points and suffixed by three exclamation points.

## Comments
Comments are not supported by JSON. A different or custom format should be used to support comments in the future.
