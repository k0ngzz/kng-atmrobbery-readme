
---

# KNG - ATM Robbery

Thank you for purchasing my ATM robbery resource. This resource was primarily made to be used with `ox_inventory`, but I have added an option to use `qb-inventory` as well. I hope you enjoy this resource. For support, please open a ticket in my discord.

## Dependencies

To ensure this script works properly, you need the following dependencies. The code for the optional resources is open if you want to modify them.

- [`qb-core`](https://github.com/qbcore-framework/qb-core)
- [`ox_lib`](https://github.com/overextended/ox_lib)
- [`ox_inventory`](https://github.com/overextended/ox_inventory) or [`qb-inventory`](https://github.com/qbcore-framework/qb-inventory)
- [`ox_target`](https://github.com/overextended/ox_target), [`qb-target`](https://github.com/qbcore-framework/qb-target) or [`interact`](https://github.com/darktrovx/interact)
- [`ps-dispatch`](https://github.com/Project-Sloth/ps-dispatch) (optional)
- [`ultra-voltlab Minigame`](https://github.com/ultrahacx/ultra-voltlab) or [`ps-ui`](https://github.com/Project-Sloth/ps-ui) (optional)

---

## Installation

### Step 1: Configure Inventory Items

#### ox_inventory

1. Go to your `ox_inventory` directory and open `items.lua` (`ox_inventory/data/items.lua`).
2. Add these items:

    ```lua
    ["towingrope"] = {
        label = "Towing Rope",
        weight = 5000,
        stack = false,
        close = true,
        description = "Towing Rope",
        durability = 100,
        consume = 0,
        client = {
            export = 'kng-atmrobbery.ATMRope'
        }
    },

    ["atmobject"] = {
        label = "ATM",
        weight = 30000,
        stack = false,
        close = true,
        description = "Stolen ATM",
        durability = 100,
        decay = true,
        consume = 0,
        server = {
            export = 'kng-atmrobbery.hackATM'
        }
    },

    ["hackingdevice"] = {
        label = "Hacking Device",
        weight = 3000,
        stack = true,
        close = true,
        decay = true,
        durability = 100,
        description = "Probably got some use to it?",
    },
    ```

#### qb-inventory

1. Go to your `qb-core` directory and open `items.lua` (`qb-core/shared/items.lua`).
2. Add these items:

    ```lua
    ['towingrope'] = {
        ['name'] = 'towingrope',
        ['label'] = 'Towing Rope',
        ['weight'] = 5000,
        ['type'] = 'item',
        ['image'] = 'towingrope.png',
        ['unique'] = true,
        ['useable'] = true,
        ['shouldClose'] = true,
        ['combinable'] = nil,
        ['description'] = 'Towing Rope'
    },

    ['atmobject'] = {
        ['name'] = 'atmobject',
        ['label'] = 'ATM',
        ['weight'] = 200,
        ['type'] = 'item',
        ['image'] = 'atmobject.png',
        ['unique'] = true,
        ['useable'] = true,
        ['shouldClose'] = true,
        ['combinable'] = nil,
        ['description'] = 'ATM'
    },

    ['hackingdevice'] = {
        ['name'] = 'hackingdevice',
        ["created"] = nil,
        ['label'] = 'Hacking Device',
        ['weight'] = 10000,
        ['type'] = 'item',
        ['image'] = 'hackingdevice.png',
        ['unique'] = false,
        ['useable'] = true,
        ['shouldClose'] = true,
        ['combinable'] = nil,
        ['description'] = 'Probably got some use to it?'
    },
    ```

---

### Step 2: Add Resources

Add the resource `kng-atmprops` to your resource folder.

---

### Step 3: Ensure Resources in Config

Make sure to start these resources after the dependencies in your `cfg`:

```plaintext
ensure kng-atmprops
ensure kng-atmrobbery
```

---

### Step 4: Add Police Alert to ps-dispatch

1. Go to `ps-dispatch/client/alerts.lua`.
2. Add this snippet at the bottom:

    ```lua
    local function ATMRobbery()
        local coords = GetEntityCoords(cache.ped)

        local dispatchData = {
            message = 'ATM Robbery',
            codeName = 'atmrobbery',
            code = '10-10',
            icon = 'fab fa-artstation',
            priority = 2,
            coords = coords,
            gender = GetPlayerGender(),
            street = GetStreetAndZone(coords),
            alertTime = nil,
            jobs = { 'leo' }
        }

        TriggerServerEvent('ps-dispatch:server:notify', dispatchData)
    end
    exports('ATMRobbery', ATMRobbery)
    ```

---

### Extra Step (ox_inventory ONLY)

If you want to carry the ATM like in the preview:

1. Download and add [`Renewed-Weaponscarry`](https://github.com/Renewed-Scripts/Renewed-Weaponscarry).
2. Go to `Renewed-Weaponscarry` directory and open `carry.lua` (`Renewed-Weaponscarry/data/carry.lua`).
3. Add this snippet at the bottom:

    ```lua
    atmobject = {
        slowMovement = 10, -- Slow player movement by percentage, 100 = can't walk, 0 = normal speed. (recommend 10-20)
        model = `kng_fleeca_atm_console`,
        pos = vec3(0.01, -0.27, 0.27),
        rot = vec3(3.0, 0.0, 0.0),
        bone = 28422,
        dict = 'anim@heists@box_carry@',
        anim = 'idle',
        disableKeys = {
            disableSprint = true,
            disableJump = true,
            disableAttack = true,
            disableDriving = true,
            disableVehicleEnter = true
        }
    },
    ```

---

And now you should be done!

---