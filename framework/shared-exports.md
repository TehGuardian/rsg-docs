---
description: Learn how to use exports to update the shared dynamically!
---

# ↗ Shared Exports

<table><thead><tr><th>Function or Event</th><th width="150" align="center">Scope</th><th width="206.79646017699116" align="center">Description</th><th align="center">returns</th></tr></thead><tbody><tr><td>exports['rsg-core']:AddItem(itemName, item)</td><td align="center">Server</td><td align="center">Accepts an item name and item object - will add new entry to RSGCore.Shared.Items</td><td align="center">boolean, string</td></tr><tr><td>exports['rsg-core']:AddJob(jobName, job)</td><td align="center">Server</td><td align="center">Accepts an job name and job object - will add new entry to RSGCore.Shared.Jobs</td><td align="center">boolean, string</td></tr><tr><td>exports['rsg-core']:AddGang(gangName, gang)</td><td align="center">Server</td><td align="center">Accepts an gang name and gang object - will add new entry to RSGCore.Shared.Gangs</td><td align="center">boolean, string</td></tr><tr><td>exports['rsg-core']:AddItems(items)</td><td align="center">Server</td><td align="center">Accepts a table or array - will loop through the table and add new entries to RSGCore.Shared.Items</td><td align="center">boolean, string, errorItem</td></tr><tr><td>exports['rsg-core']:AddJobs(jobs)</td><td align="center">Server</td><td align="center">Accepts a table or array - will loop through the table and add new entries to RSGCore.Shared.Jobs</td><td align="center">boolean, string, errorItem</td></tr><tr><td>exports['rsg-core']:AddGangs(gangs)</td><td align="center">Server</td><td align="center">Accepts a table or array - will loop through the table and add new entries to RSGCore.Shared.Gangs</td><td align="center">boolean, string, errorItem</td></tr><tr><td>RSGCore.Functions.AddItem(itemName, item)</td><td align="center">Server</td><td align="center">Accepts an item name and item object - will add new entry to RSGCore.Shared.Items</td><td align="center">boolean, string</td></tr><tr><td>RSGCore.Functions.AddJob(jobName, Job)</td><td align="center">Server</td><td align="center">Accepts an job name and job object - will add new entry to RSGCore.Shared.Jobs</td><td align="center">boolean, string</td></tr><tr><td>RSGCore.Functions.AddGang(gangName, gang)</td><td align="center">Server</td><td align="center">Accepts an gang name and gang object - will add new entry to RSGCore.Shared.Gangs</td><td align="center">boolean, string</td></tr><tr><td>RSGCore.Functions.AddItems(items)</td><td align="center">Server</td><td align="center">Accepts a table or array - will loop through the table and add new entries to RSGCore.Shared.Items</td><td align="center">boolean, string, errorItem</td></tr><tr><td>RSGCore.Functions.AddJobs(jobs)</td><td align="center">Server</td><td align="center">Accepts a table or array - will loop through the table and add new entries to RSGCore.Shared.Jobs</td><td align="center">boolean, string, errorItem</td></tr><tr><td>RSGCore.Functions.AddGangs(gangs)</td><td align="center">Server</td><td align="center">Accepts a table or array - will loop through the table and add new entries to RSGCore.Shared.Gangs</td><td align="center">boolean, string, errorItem</td></tr><tr><td>TriggerServerEvent('RSGCore:Sever:UpdateObject')</td><td align="center">Server</td><td align="center">Accepts nothing - used to update the core object (PLEASE USE THE BELOW EXAMPLE)</td><td align="center">none</td></tr><tr><td>TriggerClientEvent('RSGCore:Client:UpdateObject')</td><td align="center">Client</td><td align="center">Accepts nothing - used to update the core object (PLEASE USE THE BELOW EXAMPLE)</td><td align="center">none</td></tr></tbody></table>

## Import Jobs

* This method allows for any resource to insert job data into the shared file for the core. That means that you can make a resource, and on load, make those jobs available for use. This can be accomplished through one the ways shown below. Utilizing the function requires importing the core but using the export does not

{% hint style="danger" %}
Read over and learn the [shared.md](shared.md "mention") structure before attempting to use these
{% endhint %}

{% hint style="warning" %}
You must add this code to the client-side file of your resource when using these if you need the core object!
{% endhint %}

```lua
RegisterNetEvent('RSGCore:Client:UpdateObject', function()
	RSGCore = exports['rsg-core']:GetCoreObject()
end)
```

{% hint style="warning" %}
You must add this code to the server-side file of your resource when using these if you need the core object!
{% endhint %}

```lua
RegisterNetEvent('RSGCore:Server:UpdateObject', function()
	if source ~= '' then return false end
	RSGCore = exports['rsg-core']:GetCoreObject()
end)
```

### RSGCore.Functions.AddJob

```lua
RSGCore.Functions.AddJob(jobName --[[string]], job --[[table]])

-- Example
RSGCore.Functions.AddJob('unemployed', {
    label = 'Civilian',
    defaultDuty = true,
    offDutyPay = false,
    grades = {
        ['0'] = {
            name = 'Freelancer',
            payment = 10
        }
    }
})
```

### exports\['rsg-core']:AddJob

```lua
exports['rsg-core']:AddJob(jobName --[[string]], job --[[table]])

-- Example
exports['rsg-core']:AddJob('unemployed', {
    label = 'Civilian',
    defaultDuty = true,
    offDutyPay = false,
    grades = {
        ['0'] = {
            name = 'Freelancer',
            payment = 10
        }
    }
})
```

### RSGCore.Functions.AddJobs

```lua
RSGCore.Functions.AddJobs(jobs --[[table]])

-- Example
RSGCore.Functions.AddJobs({
    ['unemployed'] = {
        label = 'Civilian',
        defaultDuty = true,
        offDutyPay = false,
        grades = {
            ['0'] = {
                name = 'Freelancer',
                payment = 10
        }
    },
    ['trucker'] = {
	label = 'Trucker',
	defaultDuty = true,
	offDutyPay = false,
	grades = {
            ['0'] = {
                name = 'Driver',
                payment = 50
        }
    }
})
```

### exports\['rsg-core']:AddJobs

```lua
exports['rsg-core']:AddJobs(jobs --[[table]])

-- Example
exports['rsg-core']:AddJobs({
    ['unemployed'] = {
        label = 'Civilian',
        defaultDuty = true,
        offDutyPay = false,
        grades = {
            ['0'] = {
                name = 'Freelancer',
                payment = 10
        }
    },
    ['trucker'] = {
	label = 'Trucker',
	defaultDuty = true,
	offDutyPay = false,
	grades = {
            ['0'] = {
                name = 'Driver',
                payment = 50
        }
    }
})
```

## Import Gangs

* This method allows for any resource to insert gang data into the shared file for the core. That means that you can make a resource, and on load, make those gangs available for use. This can be accomplished through one the ways shown below. Utilizing the function requires importing the core but using the export does not

### RSGCore.Functions.AddGang

```lua
RSGCore.Functions.AddGang(gangName --[[string]], gang --[[table]])

-- Example
RSGCore.Functions.AddGang('azteca', {
    label = 'Azteca',
    grades = {
        ['0'] = {
            name = 'Recruit'
        }
    }
})
```

### exports\['rsg-core']:AddGang

```lua
exports['rsg-core']:AddGang(gangName --[[string]], gang --[[table]])

-- Example
exports['rsg-core']:AddGang('azteca', {
    label = 'Azteca',
    grades = {
        ['0'] = {
            name = 'Recruit'
        }
    }
})
```

### RSGCore.Functions.AddGangs

```lua
RSGCore.Functions.AddGangs(gangs --[[table]])

-- Example
RSGCore.Functions.AddGangs({
    ['lostmc'] = {
        label = 'Lost MC',
        grades = {
            ['0'] = {
                name = 'Recruit'
            }
        }
    },
    ['ballas'] = {
        label = 'Ballas',
        grades = {
            ['0'] = {
                name = 'Recruit'
            }
        }
    }
})
```

### exports\['rsg-core']:AddGangs

```lua
exports['rsg-core']:AddGangs(gangs --[[table]])

-- Example
exports['rsg-core']:AddGangs({
    ['lostmc'] = {
        label = 'Lost MC',
        grades = {
            ['0'] = {
                name = 'Recruit'
            }
        }
    },
    ['ballas'] = {
        label = 'Ballas',
        grades = {
            ['0'] = {
                name = 'Recruit'
            }
        }
    }
})
```

## Import Items

* This method allows for any resource to insert item data into the shared file for the core. That means that you can make a resource, and on load, make those items available for use. This can be accomplished through one the ways shown below. Utilizing the function requires importing the core but using the export does not

### RSGCore.Functions.AddItem

```lua
RSGCore.Functions.AddItem(itemName --[[string]], item --[[table]])

-- Example
RSGCore.Functions.AddItem('water_bottle', {
    name = 'water_bottle',
    label = 'Bottle of Water',
    weight = 10,
    type = 'item',
    image = 'water_bottle.png',
    unique = false,
    useable = true,
    shouldClose = true,
    combinable = nil,
    description = 'For all the thirsty out there'
})
```

### exports\['rsg-core']:AddItem

```lua
exports['rsg-core']:AddItem(itemName --[[string]], item --[[table]])

-- Example
exports['rsg-core']:AddItem('water_bottle', {
    name = 'water_bottle',
    label = 'Bottle of Water',
    weight = 10,
    type = 'item',
    image = 'water_bottle.png',
    unique = false,
    useable = true,
    shouldClose = true,
    combinable = nil,
    description = 'For all the thirsty out there'
})
```

### RSGCore.Functions.AddItems

```lua
RSGCore.Functions.AddItems(items --[[table]])

-- Example
RSGCore.Functions.AddItems({
    ['water_bottle'] = {
        name = 'water_bottle',
        label = 'Bottle of Water',
        weight = 10,
        type = 'item',
        image = 'water_bottle.png',
        unique = false,
        useable = true,
        shouldClose = true,
        combinable = nil,
        description = 'For all the thirsty out there'
    },
    ['sandwich'] = {
        name = 'sandwich',
        label = 'Sandwich',
        weight = 10,
        type = 'item',
        image = 'sandwich.png',
        unique = false,
        useable = true,
        shouldClose = true,
        combinable = nil,
        description = 'Nice bread for your stomach'
    }
})
```

### exports\['rsg-core']:AddItems

```lua
exports['rsg-core']:AddItems(items --[[table]])

-- Example
exports['rsg-core']:AddItems({
    ['water_bottle'] = {
        name = 'water_bottle',
        label = 'Bottle of Water',
        weight = 10,
        type = 'item',
        image = 'water_bottle.png',
        unique = false,
        useable = true,
        shouldClose = true,
        combinable = nil,
        description = 'For all the thirsty out there'
    },
    ['sandwich'] = {
        name = 'sandwich',
        label = 'Sandwich',
        weight = 10,
        type = 'item',
        image = 'sandwich.png',
        unique = false,
        useable = true,
        shouldClose = true,
        combinable = nil,
        description = 'Nice bread for your stomach'
    }
})
```