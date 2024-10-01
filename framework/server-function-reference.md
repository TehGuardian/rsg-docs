---
description: Learn about and how to use common core server functions!
---

# Server Functions

## RSGCore:Functions.GetCoords
Get the coords of a passed entity

```lua
function RSGCore.Functions.GetCoords(entity)
    local coords = GetEntityCoords(entity, false)
    local heading = GetEntityHeading(entity)
    return vector4(coords.x, coords.y, coords.z, heading)
end

-- Example

local ped = GetPlayerPed(source)
local coords = RSGCore.Functions.GetCoords(ped)
print(coords)
```

## RSGCore:Functions.GetIdentifier
Get a specific identifier of a player

```lua
function RSGCore.Functions.GetIdentifier(source, idtype)
    local idtype = idtype or RSGConfig.IdentifierType
    for key, value in pairs(GetPlayerIdentifiers(source)) do
        if string.find(value, idtype) then
            return identifier
        end
    end
    return nil
end

-- Example

local identifier = RSGCore.Functions.GetIdentifier(source, 'license')
print(identifier)

OR -- defaults to the identifier in the config of rsg-core

local identifier = RSGCore.Functions.GetIdentifier(source)
print(identifier)
```

## RSGCore:Functions.GetSource
Get a players source by identifer

```lua
function RSGCore.Functions.GetSource(identifier)
    for key, value in pairs(RSGCore.Players) do
        local identifiers = GetPlayerIdentifiers(key)
        for _, id in pairs(identifiers) do
            if identifier == id then
                return key
            end
        end
    end
    return 0
end

-- Example

local identifier = RSGCore.Functions.GetIdentifier(source, 'license')
local playerSource = RSGCore.Functions.GetSource(identifier)
print(playerSource)
```

## RSGCore:Functions.GetPlayer
Get a player by their source and access their data

```lua
function RSGCore.Functions.GetPlayer(source)
    local src = source
    if type(src) == 'number' then
        return RSGCore.Players[src]
    else
        return RSGCore.Players[RSGCore.Functions.GetSource(src)]
    end
end

-- Example

local Player = RSGCore.Functions.GetPlayer(source)
print(RSGCore.Debug(Player))

OR -- access some player data

local Player = RSGCore.Functions.GetPlayer(source)
print(Player.PlayerData.citizenid)
```

## RSGCore:Functions.GetPlayerByCitizenId
Get a player by their citizen id and access their data (must be online)

```lua
function RSGCore.Functions.GetPlayerByCitizenId(citizenid)
    for key, value in pairs(RSGCore.Players) do
        if RSGCore.Players[key].PlayerData.citizenid == citizenid then
            return RSGCore.Players[key]
        end
    end
    return nil
end

-- Example

local Player = RSGCore.Functions.GetPlayerByCitizenId('ONZ55343')
print(RSGCore.Debug(Player))

OR -- access some player data

local Player = RSGCore.Functions.GetPlayerByCitizenId('ONZ55343')
print(Player.PlayerData.license)
```

## RSGCore:Functions.GetPlayers
Get all player IDs in the server (deprecated method)

```lua
function RSGCore.Functions.GetPlayers()
    local sources = {}
    for key, value in pairs(RSGCore.Players) do
        sources[#sources + 1] = key
    end
    return sources
end

-- Example

local Players = RSGCore.Functions.GetPlayers()
print(RSGCore.Debug(Players))
```

## RSGCore:Functions.GetRSGPlayers
Access the table of all active players on the server (preferred to above)


```lua
function RSGCore.Functions.GetRSGPlayers()
    return RSGCore.Players
end

-- Example

local Players = RSGCore.Functions.GetRSGPlayers()
print(RSGCore.Debug(Players))
```

## RSGCore:Functions.CreateCallback
Creates a callback which is used on the client-side code with RSGCore.Functions.TriggerCallback

```lua
function RSGCore.Functions.CreateCallback(name, cb)
    RSGCore.ServerCallbacks[name] = cb
end

-- Example

RSGCore.Functions.CreateCallback('callbackName', function(source, cb)
    cb('Ok')
end)
```

## RSGCore:Functions.CreateUseableItem
Register an item as usable in the core

```lua
function RSGCore.Functions.CreateUseableItem(item, cb)
    RSGCore.UseableItems[item] = cb
end

-- Example

RSGCore.Functions.CreateUseableItem('example_item', function(source, item)
	local Player = RSGCore.Functions.GetPlayer(source)
	if not Player.Functions.GetItemByName(item.name) then return end
	-- Trigger code here for what item should do
end)
```

## RSGCore:Functions.CanUseItem
Check if an item is registered as usable before attempting use

```lua
function RSGCore.Functions.CanUseItem(item)
    return RSGCore.UseableItems[item]
end

-- Example

local canUse = RSGCore.Functions.CanUseItem('example_item')
print(canUse)

OR

local canUse = RSGCore.Functions.CanUseItem('example_item')
if not canUse then return end
-- Trigger code here
```

## RSGCore:Functions.UseItem
Trigger an item to be used on the player

```lua
function RSGCore.Functions.UseItem(source, item)
    RSGCore.UseableItems[item.name](source, item)
end

-- Example

local Player = RSGCore.Functions.GetPlayer(source)
if not Player.Functions.GetItemByName('example_item') then return end
RSGCore.Functions.UseItem(source, 'example_item')
```

## RSGCore:Functions.Kick
Kick a player from the server

```lua
function RSGCore.Functions.Kick(source, reason, setKickReason, deferrals)
    local src = source
    reason = '\n'..reason..'\n🔸 Check our Discord for further information: '..RSGCore.Config.Server.discord
    if setKickReason then
        setKickReason(reason)
    end
    CreateThread(function()
        if deferrals then
            deferrals.update(reason)
            Wait(2500)
        end
        if src then
            DropPlayer(src, reason)
        end
        local i = 0
        while (i <= 4) do
            i = i + 1
            while true do
                if src then
                    if(GetPlayerPing(src) >= 0) then
                        break
                    end
                    Wait(100)
                    CreateThread(function() 
                        DropPlayer(src, reason)
                    end)
                end
            end
            Wait(5000)
        end
    end)
end

-- Example

RSGCore.Functions.Kick(playerId, 'You messed up', true, true)
```

## RSGCore:Functions.AddPermission
Give a player a specific permission level (per session only)

```lua
function RSGCore.Functions.AddPermission(source, permission)
    local license = RSGCore.Functions.GetIdentifier(source, 'license')
    ExecuteCommand(('add_principal identifier.%s rsgcore.%s'):format(license, permission))
    RSGCore.Commands.Refresh(source)
end

-- Example

local Player = RSGCore.Functions.GetPlayer(playerId)
local permission = 'admin'
RSGCore.Functions.AddPermission(Player.PlayerData.source, permission)
```

## RSGCore:Functions.RemovePermission
Remove all of the players permissions on the server (per session only)

```lua
function RSGCore.Functions.RemovePermission(source, permission)
    local src = source
    local license = RSGCore.Functions.GetIdentifier(src, 'license')
    if permission then
        if IsPlayerAceAllowed(src, permission) then
            ExecuteCommand(('remove_principal identifier.%s rsgcore.%s'):format(license, permission))
            RSGCore.Commands.Refresh(src)
        end
    else
        for k,v in pairs(RSGCore.Config.Server.Permissions) do
            if IsPlayerAceAllowed(src, v) then
                ExecuteCommand(('remove_principal identifier.%s rsgcore.%s'):format(license, v))
                RSGCore.Commands.Refresh(src)
            end
        end
    end
end

-- Example

local Player = RSGCore.Functions.GetPlayer(playerId)
local permission = 'admin'
RSGCore.Functions.RemovePermission(Player.PlayerData.source, permission)
```

## RSGCore:Functions.HasPermission
Check if a player has the permission level needed

```lua
function RSGCore.Functions.HasPermission(source, permission)
    if IsPlayerAceAllowed(source, permission) then return true end
    return false
end

-- Example

local hasPerms = RSGCore.Functions.HasPermission(source, 'admin')
print(hasPerms)

OR -- using as a condition

local hasPerms = RSGCore.Functions.HasPermission(source, 'admin')
if not hasPerms then return end
-- Trigger code here
```

## RSGCore:Functions.GetPermission
Get a player's permission level

```lua
function RSGCore.Functions.GetPermission(source)
    local src = source
    local perms = {}
    for key, value in pairs (RSGCore.Config.Server.Permissions) do
        if IsPlayerAceAllowed(src, value) then
            perms[value] = true
        end
    end
    return perms
end

-- Example

local permissions = RSGCore.Functions.GetPermission(source)
print(RSGCore.Debug(permissions))

OR -- using as a condition

local permissions = RSGCore.Functions.GetPermission(source)
for key, value in pairs(permissions) do
    if value == true then
        print(key)
    end
end
```

## RSGCore:Functions.IsPlayerBanned
Check if a player is banned (used for connection)

```lua
function RSGCore.Functions.IsPlayerBanned(source)
    local plicense = RSGCore.Functions.GetIdentifier(source, 'license')
    local result = MySQL.Sync.fetchSingle('SELECT * FROM bans WHERE license = ?', { plicense })
    if not result then return false end
    if os.time() < result.expire then
        local timeTable = os.date('*t', tonumber(result.expire))
        return true, 'You have been banned from the server:\n' .. result.reason .. '\nYour ban expires ' .. timeTable.day .. '/' .. timeTable.month .. '/' .. timeTable.year .. ' ' .. timeTable.hour .. ':' .. timeTable.min .. '\n'
    else
        MySQL.Async.execute('DELETE FROM bans WHERE id = ?', { result.id })
    end
    return false
end

-- Example

local isBanned = RSGCore.Functions.IsPlayerBanned(source)
print(isBanned)
```

## RSGCore:Functions.IsLicenseInUse
Prevent duplicate licenses on the server (used for connection)

```lua
function RSGCore.Functions.IsLicenseInUse(license)
    local players = GetPlayers()
    for key, value in pairs(players) do
        local identifiers = GetPlayerIdentifiers(value)
        for _, id in pairs(identifiers) do
            if string.find(id, 'license') then
                if id == license then
                    return true
                end
            end
        end
    end
    return false
end

-- Example

local license = RSGCore.Functions.GetIdentifier(source, 'license')
local isUsed = RSGCore.Functions.IsLicenseInUse(license)
print(isUsed)
```
