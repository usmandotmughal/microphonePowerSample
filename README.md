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
local microphone = require "plugin.microphonePower"
local json = require("json")

local function isValueInTable( haystack, needle )
	assert( type(haystack) == "table", "isValueInTable() : First parameter must be a table." )
	assert( needle ~= nil, "isValueInTable() : Second parameter must be not be nil." )

	for key, value in pairs( haystack ) do
		if ( value == needle ) then
			return true
		end
	end
	return false
end

local function canUseMicrophone()
	local grantedPermissions = system.getInfo("grantedAppPermissions")
	if ( grantedPermissions ) then
		if ( not isValueInTable( grantedPermissions, "Microphone" ) ) then
			return false
		end
	end
	return true 
end

local function onMicrophoneAccessGiven()
	timer.performWithDelay( 100, function()
		microphone.getPower()
	end, -1 )
end

-- Called when the user has granted or denied the requested permissions.
local function permissionsListener( event )
	-- Print out granted/denied permissions.
	print( "permissionsListener( " .. json.prettify( event or {} ) .. " )" )

	if canUseMicrophone() then
		print("permissionsListener : microphone access given")
		-- microphone access is given 
		-- now get power of microphone
		onMicrophoneAccessGiven()
	else
		-- ask for permissions again to access microphone
		print("permissionsListener : microphone access not given")
	end
end

if canUseMicrophone() then
 	-- microphone access is given 
	-- now get power of microphone
	onMicrophoneAccessGiven()
else
	-- Make the actual request from the user.
	native.showPopup( "requestAppPermission", {
		appPermission = "Microphone",
		urgency = "Critical",
		rationaleTitle = "Microphone Permissions Required!",
		rationaleDescription = "This app wants to access your microphone.",
		listener = permissionsListener,
	} )
end

local function callBackListener( event )
	print( "event in microphonePower callback : ", json.encode(event) )
end
-- init microphonePower plugin
microphone.init(callBackListener)

```
