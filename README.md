# Microphone Power Sample

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
