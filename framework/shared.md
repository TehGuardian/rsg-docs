---
description: Learn how to manage your server's jobs, vehicles, items, etc!
---

# 📜 Shared

## Introduction

* The shared file inside rsg-core contains all the information for your jobs, vehicles, items & more! You will spend a lot of time in this file configuring everything to your exact specifications. Jenkins hashes are used frequently, you can find more information on those [here](https://cookbook.fivem.net/2019/06/23/lua-support-for-compile-time-jenkins-hashes/)

{% hint style="success" %}
RSGCore uses lua tables to store static information instead of a database to prevent needing to execute queries. When you combine this with the ability to use [shared-exports.md](shared-exports.md "mention") it can be a very powerful tool!
{% endhint %}

### Utility

* Found in rsg-core/shared/main.lua

```lua
RSGShared = RSGShared or {}

local StringCharset = {}
local NumberCharset = {}

for i = 48,  57 do NumberCharset[#NumberCharset+1] = string.char(i) end
for i = 65,  90 do StringCharset[#StringCharset+1] = string.char(i) end
for i = 97, 122 do StringCharset[#StringCharset+1] = string.char(i) end

function RSGShared.RandomStr(length)
    if length <= 0 then return '' end
    return RSGShared.RandomStr(length - 1) .. StringCharset[math.random(1, #StringCharset)]
end

function RSGShared.RandomInt(length)
    if length <= 0 then return '' end
    return RSGShared.RandomInt(length - 1) .. NumberCharset[math.random(1, #NumberCharset)]
end

function RSGShared.SplitStr(str, delimiter)
    local result = {}
    local from = 1
    local delim_from, delim_to = string.find(str, delimiter, from)
    while delim_from do
		result[#result+1] = string.sub(str, from, delim_from - 1)
        from = delim_to + 1
        delim_from, delim_to = string.find(str, delimiter, from)
    end
	result[#result+1] = string.sub(str, from)
    return result
end

function RSGShared.Trim(value)
	if not value then return nil end
    return (string.gsub(value, '^%s*(.-)%s*$', '%1'))
end

function RSGShared.Round(value, numDecimalPlaces)
    if not numDecimalPlaces then return math.floor(value + 0.5) end
    local power = 10 ^ numDecimalPlaces
    return math.floor((value * power) + 0.5) / (power)
end

function RSGShared.ChangeVehicleExtra(vehicle, extra, enable)
	if DoesExtraExist(vehicle, extra) then
		if enable then
			SetVehicleExtra(vehicle, extra, false)
			if not IsVehicleExtraTurnedOn(vehicle, extra) then
				RSGShared.ChangeVehicleExtra(vehicle, extra, enable)
			end
		else
			SetVehicleExtra(vehicle, extra, true)
			if IsVehicleExtraTurnedOn(vehicle, extra) then
				RSGShared.ChangeVehicleExtra(vehicle, extra, enable)
			end
		end
	end
end

function RSGShared.SetDefaultVehicleExtras(vehicle, config)
    -- Clear Extras
    for i = 1,20 do
        if DoesExtraExist(vehicle, i) then
            SetVehicleExtra(vehicle, i, 1)
        end
    end

    for id, enabled in pairs(config) do
        RSGShared.ChangeVehicleExtra(vehicle, tonumber(id), type(enabled) == 'boolean' and enabled or true)
    end
end

RSGShared.StarterItems = {
    ['consumable_bread_roll'] = { amount = 1, item = 'consumable_bread_roll' },
    ['consumable_water_filtered'] = { amount = 1, item = 'consumable_water_filtered' },
}
```

### Items

* Found in rsg-core/shared/items.lua

```lua
RSGShared.Items = {
    ['id_card'] = {
        ['name'] = 'id_card', -- Actual item name for spawning/giving/removing
        ['label'] = 'ID Card', -- Label of item that is shown in inventory slot
        ['weight'] = 0, -- How much the items weighs
        ['type'] = 'item', -- What type the item is (ex: item, weapon)
        ['image'] = 'id_card.png', -- This item image that is found in rsg-inventory/html/images (must be same name as ['name'] from above)
        ['unique'] = true, -- Is the item unique (true|false) - Cannot be stacked & accepts item info to be assigned
        ['useable'] = true, -- Is the item useable (true|false) - Must still be registered as useable
        ['shouldClose'] = false, -- Should the item close the inventory on use (true|false)
        ['combinable'] = nil, -- Is the item able to be combined with another? (nil|table)
        ['description'] = 'A card containing all your information to identify yourself' -- Description of time in inventory
    }
}

-- Example of an item that is combinable and not nil

['combinable'] = {
    accept = {'wheat'}, -- The other item that can be it can be combined with
    reward = 'consumable_bread_roll', -- The item that is rewarded upon successful combine
    anim = { -- Set the animation, progressbar text and length of time it takes to combine
        ['dict'] = '', -- The animation dictionary
        ['lib'] = '', -- The animation library
        ['text'] = 'Crafting', -- Text that will be displayed in the progress bar
        ['timeOut'] = 15000,} -- How long the animation should take to complete
    }
}
```

### Jobs

* Found in rsg-core/shared/jobs.lua

```lua
RSGShared.ForceJobDefaultDutyAtLogin = true -- true: Force duty state to jobdefaultDuty | false: set duty state from database last saved
RSGShared.Jobs = {
    ['unemployed'] = {
        label = 'Civilian',
        defaultDuty = true,
        offDutyPay = false,
        grades = {
            ['0'] = { name = 'Freelancer', payment = 5 },
        },
    },
    ['vallaw'] = {
        label = 'Valentine Law Enforcement',
        type = 'leo',
        defaultDuty = false,
        offDutyPay = false,
        grades = {
            ['0'] = { name = 'Recruit', payment = 10 },
            ['1'] = { name = 'Deputy', payment = 25 },
            ['2'] = { name = 'Sheriff', isboss = true, payment = 50 },
        },
    },
}
```

### Vehicles

* Found in rsg-core/shared/vehicles.lua

```lua
RSGShared.Vehicles = {
    ['CART01'] = {
        ['name'] = 'Wooden Cart 1',
        ['brand'] = '?',
        ['model'] = 'CART01',
        ['price'] = 10,
        ['category'] = 'carts',
        ['hash'] = '-824257932',
        ['shop'] = 'cart',
    },
}
```

### Gangs

* Found in rsg-core/shared/gangs.lua

```lua
RSGShared.Gangs = {
    ['example'] = {
        label = "Example Gang",
        grades = {
            ['0'] = {
                name = 'Recruit'
            },
            ['1'] = {
                name = 'Enforcer'
            },
            ['2'] = {
                name = 'Shot Caller'
            },
            ['3'] = {
                name = 'Boss',
                isboss = true
            },
        }
    },
}
```

### Weapons

* Found in rsg-core/shared/weapons.lua

{% hint style="warning" %}
Weapons are added in shared/items.lua as well!
{% endhint %}

```lua
RSGShared.Weapons = {
    [`weapon_revolver_navy`] = { -- Weapon hash (uses compile-time Jenkins hashes - See link at bottom of page)
        ['name'] = 'weapon_revolver_navy', -- Actual item name for spawning/giving/removing
        ['label'] = 'Navy', -- Label of item that is shown in inventory slot
        ['weight'] = 1000, -- How much the items weighs
        ['type'] = 'weapon', -- What type the item is (ex: item, weapon)
        ['ammotype'] = 'AMMO_REVOLVER', -- The type of ammo this weapon accepts
        ['image'] = 'weapon_revolver_navy.png', -- This item image that is found in rsg-inventory/html/images (must be same name as ['name'] from above)
        ['unique'] = true, -- Is the item unique (true|false) - Cannot be stacked & accepts item info to be assigned
        ['useable'] = false, -- Is the item useable (true|false) - Must still be registered as useable
        ['description'] = 'A small firearm designed to be held in one hand' -- Description of time in inventory
    }
}
```