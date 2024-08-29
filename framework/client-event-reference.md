---
description: Learn about and how to use common core client events!
---

# Client Event Reference

### RSGCore:Client:OnPlayerLoaded

* Handles the player loading in after character selection

{% hint style="success" %}
This event can be used as an event handler to trigger code because it signifies the player has successfully loaded into the server!
{% endhint %}

```lua
RegisterNetEvent('RSGCore:Client:OnPlayerLoaded', function()
    print('Im a client and i just loaded into your server!')
end)
```

### RSGCore:Client:OnPlayerUnload

* Handles the player login out to character selection

{% hint style="success" %}
This event can be used as an event handler to trigger code because it signifies the player has successfully unloaded or logged out of the server!
{% endhint %}

```lua
RegisterNetEvent('RSGCore:Client:OnPlayerUnload', function()
    print('Im a client and i just logged out of your server!')
end)
```

### RSG-Appearance:Client:OpenCreator

* Handles the event that triggers when the "Create New Character" button is pressed.

{% hint style="success" %}
This event it triggers as soon as the player selects the option to create a new character. {% endhint %}
{% endhint %}

```lua
RegisterNetEvent('rsg-appearance:client:OpenCreator', function()
    print('Im a client and i just logged out of your server!')
end)
```

### RSG-Spawn:Client:NewPlayer

* Handles the event triggered after the player creates their character.

{% hint style="success" %}
Triggered after the player creates their character. This event is useful for executing actions or displaying UI elements immediately after a player has created their new character.
{% endhint %}

```lua
RegisterNetEvent('rsg-spawn:client:newplayer', function()
    print('Im a client and i just logged out of your server!')
end)
```

### RSGCore:Command:SpawnVehicle
Arguments = WagonName

Client example

-- /spawnveh chuckwagon000x
```lua
RegisterCommand('spawnveh', function(_, args)
    local vehicle = RSGCore.Shared.Trim(args[1])
    TriggerEvent('RSGCore:Command:SpawnVehicle', vehicle)
end)
```

Server example

-- /spawnveh chuckwagon000x
```lua
RegisterCommand('spawnveh', function(source, args)
    local vehicle = RSGCore.Shared.Trim(args[1])
    TriggerClientEvent('RSGCore:Command:SpawnVehicle', source, vehicle)
end)
```

### RSGCore:Command:SpawnHorse
Arguments = HorseName

Client example

-- /spawnhorse a_c_horse_tennesseewalker_redroan
```lua
RegisterCommand('spawnhorse', function(_, args)
    local vehicle = RSGCore.Shared.Trim(args[1])
    TriggerEvent('RSGCore:Command:SpawnHorse', vehicle)
end)
```

Server example
-- /spawnhorse a_c_horse_tennesseewalker_redroan
```lua
RegisterCommand('spawnhorse', function(source, args)
    local vehicle = RSGCore.Shared.Trim(args[1])
    TriggerClientEvent('RSGCore:Command:SpawnHorse', source, vehicle)
end)
```


## RSGCore:Command:DeleteVehicle

Client example
```lua
RegisterCommand('deleteveh', function(_, args)
    TriggerEvent('RSGCore:Command:DeleteVehicle')
end)
```

Server example
```lua
RegisterCommand('deleteveh', function(source, args)
    TriggerClientEvent('RSGCore:Command:DeleteVehicle', source)
end)
```

## RSGCore:Player:SetPlayerData
-- This event can be used as an event handler to trigger code because it indicates that the players data has changed!

```lua
RegisterNetEvent('RSGCore:Player:SetPlayerData', function(val)
    PlayerData = val
    print(RSGCore.Debug(PlayerData))
end)
```

## RSGCore:Notify

-- Arguments = message, type, length

Client Side Example
```lua
-- /testnotify This is my message, primary, 5000

RegisterCommand('testnotify', function(_, args)
    local message = {}
    for i=1, #args do
        message[#message + 1] = args[i]
        if string.match(args[i], ',') then
            local text = table.concat(message, ' '):gsub(",", "")
            local type = args[i + 1]:gsub(",", "")
            local length = args[i + 2]
            TriggerEvent('RSGCore:Notify', text, type, length)
            break
        end
    end
end)

-- Using RSGCore's shared functions!
RegisterCommand('testnotify', function(_, args)
    local message = RSGCore.Shared.SplitStr(table.concat(args, ' '), ",")
    local text = message[1]
    local type = RSGCore.Shared.Trim(message[2])
    local length = tonumber(message[3])
    TriggerEvent('RSGCore:Notify', text, type, length)
end)
```

Server Side example

```lua
-- /testnotify This is my message, primary, 5000

RegisterCommand('testnotify', function(source, args)
    local message = {}
    for i=1, #args do
        message[#message + 1] = args[i]
        if string.match(args[i], ',') then
            local text = table.concat(message, ' '):gsub(",", "")
            local type = args[i + 1]:gsub(",", "")
            local length = args[i + 2]
            TriggerClientEvent('RSGCore:Notify', source, text, type, length)
            break
        end
    end
end)

-- Using RSGCore's shared functions!

RegisterCommand('testnotify', function(source, args)
    local message = RSGCore.Shared.SplitStr(table.concat(args, ' '), ",")
    local text = message[1]
    local type = RSGCore.Shared.Trim(message[2])
    local length = tonumber(message[3])
    TriggerClientEvent('RSGCore:Notify', source, text, type, length)
end)
```

## RSGCore:Client:UpdateObject
This event must be used as a handler when using Shared Exports because it refreshes the core object in your resource

```lua
RegisterNetEvent('RSGCore:Client:UpdateObject', function()
    RSGCore = exports['rsg-core']:GetCoreObject()
end)
```
