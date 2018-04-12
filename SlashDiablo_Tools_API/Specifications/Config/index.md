# Configuration

Various modules may need to be configured by the user in order to change the way they behave.

## History

User configurations in BH were very flexible, offering the ability to customize the way the maphack would work inside of the BH.cfg file. It can read key bindings, key toggles, and can prioritize configuration settings. Further work was done to introduce the ability to reload the configuration file, in the case that the user modified it. However, any changes to the configuration (via UI or toggles) are not saved to the file.

## The Problem

The configuration file must be named BH.cfg. No other differently named configuration file is used. There is no ingame write command.
