# Karma Developments - Snr Buns Restaurant - by ANTUNES 

- Hello, first of all thank you for purchasing our script !

- Feel free to open a support ticket to resolve your problem/question. - Karma Developments - 

- Make sure to run `ox_lib`, `interact`, `karma-npcdialog` and `karma-jobtasksUI` before `karma-snrbuns`
- To download `interact` you can find it at `free-releases` channel in our Discord!

# Items to be added to QBCore `qb-core/shared/items.lua` :

```lua

['karma_snrbuns_drink']    = { name = 'karma_snrbuns_drink', label = 'Drink', weight = 75, type = 'item', image = 'karma_snrbuns_drink.png', unique = false, useable = false, shouldClose = false, combinable = nil, description = ''},
['karma_snrbuns_main']     = { name = 'karma_snrbuns_main',  label = 'Main',  weight = 75, type = 'item', image = 'karma_snrbuns_main.png',  unique = false, useable = false, shouldClose = false, combinable = nil, description = ''},
['karma_snrbuns_side']     = { name = 'karma_snrbuns_side',  label = 'Fries',  weight = 75, type = 'item', image = 'karma_snrbuns_side.png',  unique = false, useable = false, shouldClose = false, combinable = nil, description = ''},
['karma_snrbuns_box']      = { name = 'karma_snrbuns_box',   label = 'Food Container',   weight = 75, type = 'item', image = 'karma_snrbuns_box.png',   unique = true, useable = false, shouldClose = false, combinable = nil, description = ''},
['karma_snrbuns_payslip']      = { name = 'karma_snrbuns_payslip',   label = 'Work Payslip',   weight = 75, type = 'item', image = 'karma_snrbuns_payslip.png',   unique = false, useable = false, shouldClose = false, combinable = nil, description = ''},

```

# Items to be added to OX INVENTORY `ox_inventory/data/items.lua`

```lua

    ['karma_snrbuns_box'] = {
        label = 'Food Container',
        weight = 1,
        stack = false,
    },

    ['karma_snrbuns_payslip'] = {
        label = 'Work Payslip',
        description = '$250 each paycheck\n (You can exchange them at the bank)',
        weight = 1,
        stack = true,
    },

    ['karma_snrbuns_main'] = {
        label = 'Burger',
        weight = 1,
        dropmodel = 'prop_food_bs_bag_01', --- Custom drop model
        client = {
            status = { hunger = 200000 },
            anim = 'eating',
            prop = 'burger',
            usetime = 2500,
            notification = 'You ate a delicious burger'
        },
    },

    ['karma_snrbuns_side'] = {
        label = 'Fries',
        weight = 1,
        dropmodel = 'prop_food_bs_bag_01',
        client = {
            status = { hunger = 200000 },
            anim = { 
                dict = 'mp_player_inteat@burger',
                clip = 'mp_player_int_eat_burger'
            },
            prop = {
                model = `prop_food_bs_chips`,
                bone = 60309, -- mão esquerda
                pos = vec3(0.10, 0.02, -0.09),
                rot = vec3(55.0, 0.0, 270.0)
            },
            usetime = 2500,
            notification = 'You ate some fries!'
        },
    },

    ['karma_snrbuns_drink'] = {
        label = 'Drink',
        weight = 1,
        dropmodel = 'prop_cs_bs_cup',
        client = {
            status = { thirst = 200000 },
            anim = { dict = 'amb@world_human_drinking@coffee@male@idle_a', clip = 'idle_c' },
            prop = { 
                model = `prop_cs_bs_cup`, 
                bone = 57005, 
                pos = vec3(0.12, 0.02, 0.0), 
                rot = vec3(-90.0, 0.0, 0.0) 
            },
            usetime = 2500,
            notification = 'You quenched your thirst with a sprunk'
        },
    },

```

# Add this to your Karma NPC Dialog System in `shared/config.lua`

```lua

{
        name = "Mark Bankerman", -- NPC name
		hide = false, --Hide NPC from the tablet list 
        private = false, --If you wanna make the NPC Private, meaning it wont show on the Tablet unless they speak with him!
        text = "Hey there, do you want to exchange your paychecks? ", -- Initial dialogue text
        domain = "Banker", -- Domain / Domain is what the reputation system relies on
        ped = "cs_movpremmale", -- NPC Model
        scenario = "WORLD_HUMAN_GUARD_STAND", -- NPC Scenario
        police = true, -- Indicates whether the NPC is accessible to Police workers
        coords = vector4(243.0616, 224.3393, 105.2868, 166.7854), -- Coordinates
               options = {
            {
                label = "Exchange Paychecks",
                requiredrep = 0,
                type = "server",
                event = "karma-snrbuns:server:exchangePayslips",
                args = {} 
                
            },
            {
                label = "Leave conversation",
                type = "command",
                event = "e c",
                args = {} 
            },
        }
    },
    {
        name = "Mitch Bun", -- NPC name
		hide = false, --Hide NPC from the tablet list 
        private = false, --If you wanna make the NPC Private, meaning it wont show on the Tablet unless they speak with him!
        text = "Hey there, I don't think we've crossed paths before. I'm Mitch, and i'm in charge of the Snr Buns. You thinkin' about joining our crew?", -- Initial dialogue text
        domain = "Snr Bun Worker", -- Domain / Domain is what the reputation system relies on
        ped = "csb_burgerdrug", -- NPC Model
        scenario = "WORLD_HUMAN_CLIPBOARD", -- NPC Scenario
        police = true, -- Indicates whether the NPC is accessible to Police workers
        coords = vector4(-504.2564, -697.1109, 32.6717, 270.9183), -- Coordinates
        options = { -- Dialogue options available when interacting with the NPC
            {
                label = "I want to work", -- Option text
                requiredrep = 0, -- Required reputation to select this option
                type = "add", -- Type: command/client/server/add ~ In this case add means that this btn would bring more dialogue
                event = "",
                data = { -- Additional data provided when this option is chosen
                    text = "Ready for a day of hard work?",
                    options = {
                        {
                            label = "Sign in/Out", -- Subsequent option
                            event = "snrbuns:client:openBossMenu", -- Event triggered by this option
                            type = "client", -- Interaction type
                            args = {} -- Arguments passed to the event
                        },
                        {
                            label = "Work Clothes", -- Option to leave the conversation
                            event = "", -- No event triggered
                            type = "none", -- Indicates no further interaction
                            args = {} -- No arguments
                        },
                                                {
                            label = "Leave conversation", -- Option to leave the conversation
                            event = "e c", -- No event triggered
                            type = "command", -- Indicates no further interaction
                            args = {} -- No arguments
                        },
                    }
                },
                args = {} 
            },
            {
                label = "Leave conversation", -- Simple option to exit interaction
                requiredrep = 0, -- No reputation requirement
                type = "command", -- Type: command/client/server/add
                event = "e c", -- Event triggered
                args = {}
            },
        }
    },
    
```


# Discord: ANTUNES 
# Karma Discord Discord : https://discord.gg/zg7cRhB4Yh
# Copyright (c) 2023-2024, Karma Developments Ltd. All rights reserved.