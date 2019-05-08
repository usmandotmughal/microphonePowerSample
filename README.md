# Microphone Power Sample

### Add permissions in build.settings file

#### Android
```
settings =
{
    android =
    {
        usesPermissions =
        {
            "android.permission.RECORD_AUDIO",
        },
    },
}
```
#### iOS
```
settings =
{
    iphone =
    {
        plist =
        {
            NSMicrophoneUsageDescription = "This app would like to access the microphone.",
        },
    },
}
```

#### Main.lua
```
local micrphone = require "plugin.micrphonePower"

local json = require("json")

local function listener( event )
	print( "event : ", json.encode(event) )
end

micrphone.init(listener)

timer.performWithDelay( 100, function()
	micrphone.getPower()
end, -1 )
```
