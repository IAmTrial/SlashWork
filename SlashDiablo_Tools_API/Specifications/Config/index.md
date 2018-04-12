# Configuration

Various modules may need to be configured by the user in order to change the way they behave.

## History

User configurations in BH were very flexible, offering the ability to customize the way the maphack would work inside of the BH.cfg file. It can read key bindings, key toggles, and can prioritize configuration settings. Further work was done to introduce the ability to reload the configuration file, in the case that the user modified it. However, any changes to the configuration (via UI or toggles) are not saved to the file.

## The Problem

The configuration file must be named BH.cfg. No other differently named configuration file is used. There is no ingame write command.

## The Solution

Enable different configuration files to be read in. Implement a write to file command.

## Implementation

The next sections require the JSON parsing library [JSON for Modern C++](https://github.com/nlohmann/json). The biggest reasons for using this over the original configuration parser is due to this parser being fully featured, heavily tested, and highly efficient. The original parser is difficult to develop, but uses a format that is so similar to the JSON format. This also replaces JSONObject in the old BH project.

The class Config file is created with the name of the configuration file. Reading from the file does not occur when this Config is initialized. The file extension should be “.json,” but this does not have to be enforced.

A member function is dedicate to reading and interpreting the configuration file. First, the parser reads and interprets the file into JSON format. Afterwards, the individual modules can extract the configuration settings.

Each JSON entry will utilize one of the following mappings:

### Value
```JSON
“GameListRefresh”: 1500,
“Quest Drop Warning”: false
```

### Key Binding
```JSON
“Reload Config”: “VK_NUMPAD0”,
```

### Key Toggle
```JSON
“Stats on Right”: {
    “Value”: False,
    “Key”: “None”
}
```

### Array
```JSON
“AutomapInfo”: [
    “Name: %GAMENAME%”,
    “Password: %GAMEPASS%”
]
```

### Mapping
```JSON
“Missile Color”: {
    “Player”: 0x97,
    “Neutral”: 0x0A,
    “Partied”: 0x84,
    “Hostile”: 0x5B
}
```

The function to write to a file uses this format, 


