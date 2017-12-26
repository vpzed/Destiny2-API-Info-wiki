# Destiny 2 API Introduction - Part 5 - Items

## Introduction

In this article we will exercise some of the item concepts from Part 4.  As this is an article series for beginners we will stick to some introductory examples to give an idea of how things work.  The API responses for items can be very large so sometimes I'll be showing snippets of the responses which focus on the topic being discussed - this will be indicated with {snip}.

## Inventory Example

Going back to our example player Dattowatto we can see the equipped items of that character by choosing a different component from Destiny2.GetCharacter than we did in Part 2.

https://www.bungie.net/Platform/Destiny2/2/Profile/4611686018428389623/Character/2305843009262373961/?components=205

    {
        "Response": {
            "equipment": {
                "data": {
                    "items": [
                        {
                            "itemHash": 1960218487,
                            "itemInstanceId": "6917529040022201988",
                            "quantity": 1,
                            "bindStatus": 0,
                            "location": 1,
                            "bucketHash": 1498876634,
                            "transferStatus": 1,
                            "lockable": true,
                            "state": 0
                        },
                        {
                            "itemHash": 3854359821,
                            "itemInstanceId": "6917529035517937640",
                            "quantity": 1,
                            "bindStatus": 0,
                            "location": 1,
                            "bucketHash": 2465295065,
                            "transferStatus": 1,
                            "lockable": true,
                            "state": 1
                        },
                        {
                            "itemHash": 1508896098,
                            "itemInstanceId": "6917529034078959643",
                            "quantity": 1,
                            "bindStatus": 0,
                            "location": 1,
                            "bucketHash": 953998645,
                            "transferStatus": 1,
                            "lockable": true,
                            "state": 1
                        },
                        {
                            "itemHash": 1804445917,
                            "itemInstanceId": "6917529038205026946",
                            "quantity": 1,
                            "bindStatus": 0,
                            "location": 1,
                            "bucketHash": 3448274439,
                            "transferStatus": 1,
                            "lockable": true,
                            "state": 1
                        },
                        {
                            "itemHash": 691332172,
                            "itemInstanceId": "6917529038485499802",
                            "quantity": 1,
                            "bindStatus": 0,
                            "location": 1,
                            "bucketHash": 3551918588,
                            "transferStatus": 1,
                            "lockable": true,
                            "state": 1
                        },
                        {
                            "itemHash": 3865618708,
                            "itemInstanceId": "6917529038562390852",
                            "quantity": 1,
                            "bindStatus": 0,
                            "location": 1,
                            "bucketHash": 14239492,
                            "transferStatus": 1,
                            "lockable": true,
                            "state": 1
                        },
                        {
                            "itemHash": 1337167606,
                            "itemInstanceId": "6917529038179232715",
                            "quantity": 1,
                            "bindStatus": 0,
                            "location": 1,
                            "bucketHash": 20886954,
                            "transferStatus": 1,
                            "lockable": true,
                            "state": 1
                        },
                        {
                            "itemHash": 1197579132,
                            "itemInstanceId": "6917529029542792933",
                            "quantity": 1,
                            "bindStatus": 0,
                            "location": 1,
                            "bucketHash": 1585787867,
                            "transferStatus": 1,
                            "lockable": true,
                            "state": 0
                        },
                        {
                            "itemHash": 997920961,
                            "itemInstanceId": "6917529031221707329",
                            "quantity": 1,
                            "bindStatus": 0,
                            "location": 1,
                            "bucketHash": 4023194814,
                            "transferStatus": 1,
                            "lockable": true,
                            "state": 0
                        },
                        {
                            "itemHash": 807458183,
                            "itemInstanceId": "6917529029236363498",
                            "quantity": 1,
                            "bindStatus": 0,
                            "location": 1,
                            "bucketHash": 2025709351,
                            "transferStatus": 1,
                            "lockable": true,
                            "state": 0
                        },
                        {
                            "itemHash": 96858972,
                            "itemInstanceId": "6917529040028065789",
                            "quantity": 1,
                            "bindStatus": 0,
                            "location": 1,
                            "bucketHash": 284967655,
                            "transferStatus": 1,
                            "lockable": true,
                            "state": 0
                        },
                        {
                            "itemHash": 2958378809,
                            "itemInstanceId": "6917529029206452268",
                            "quantity": 1,
                            "bindStatus": 0,
                            "location": 1,
                            "bucketHash": 3284755031,
                            "transferStatus": 3,
                            "lockable": false,
                            "state": 0
                        },
                        {
                            "itemHash": 3115055261,
                            "itemInstanceId": "6917529035710821166",
                            "quantity": 1,
                            "bindStatus": 0,
                            "location": 1,
                            "bucketHash": 4274335291,
                            "transferStatus": 3,
                            "lockable": true,
                            "state": 0
                        },
                        {
                            "itemHash": 1954221115,
                            "itemInstanceId": "6917529029213981834",
                            "quantity": 1,
                            "bindStatus": 0,
                            "location": 1,
                            "bucketHash": 3054419239,
                            "transferStatus": 3,
                            "lockable": true,
                            "state": 0
                        },
                        {
                            "itemHash": 3282648591,
                            "itemInstanceId": "6917529047358959675",
                            "quantity": 1,
                            "bindStatus": 0,
                            "location": 1,
                            "bucketHash": 1269569095,
                            "transferStatus": 3,
                            "lockable": false,
                            "state": 0
                        }
                    ]
                },
                "privacy": 1
            },
            "itemComponents": {}
        },
        "ErrorCode": 1,
        "ThrottleSeconds": 0,
        "ErrorStatus": "Success",
        "Message": "Ok",
        "MessageData": {}
    }

You can see that even this basic equipped items query returns a pretty large response.  Lets take a look at the first object in Response.equipment.data.items:

    {
        "itemHash": 1960218487,
        "itemInstanceId": "6917529040022201988",
        "quantity": 1,
        "bindStatus": 0,
        "location": 1,
        "bucketHash": 1498876634,
        "transferStatus": 1,
        "lockable": true,
        "state": 0
    }

The key items in this object are bucketHash, itemHash, and itemInstanceId.  The bucketHash tells us about the item type, the itemHash tells us about the base item, and itemInstanceHash tells us about the instanced item - concepts we went over in the Part 4.


## Static/Base Item Information

So using the base item information above we query the game manifest table DestinyInventoryBucketDefinition for bucketHash: 1498876634 using the techniques described in Part 3.

    {
        "displayProperties": {
            "description": "Weapons that deal kinetic damage. Most effective when dealing with unshielded enemies.",
            "name": "Kinetic Weapons",
            "hasIcon": false
        },
        "scope": 0,
        "category": 3,
        "bucketOrder": 20,
        "itemCount": 10,
        "location": 1,
        "hasTransferDestination": true,
        "enabled": true,
        "fifo": false,
        "hash": 1498876634,
        "index": 0,
        "redacted": false,
        "blacklisted": false
    }

Here we can see that this item is a Kinetic Weapon.

Then we query the manifest table DestinyInventoryItemDefinition for itemHash: 1960218487.  The following is a heavily snipped version of the response since it is over 700 lines normally and we'll concentrate on the basics in this first example.

    {
        "displayProperties": {
            "description": "Strange things wake at the stroke of twelve.",
            "name": "Nameless Midnight",
            "icon": "/common/destiny2_content/icons/535cb2c4046bec8ca7af199babef875d.jpg",
            "hasIcon": true
        },
        "backgroundColor": {
          {snip}
        },
        "screenshot": "/common/destiny2_content/screenshots/1960218487.jpg",
        "itemTypeDisplayName": "Scout Rifle",
        "uiItemDisplayStyle": "",
        "itemTypeAndTierDisplayName": "Legendary Scout Rifle",
        "displaySource": "",
        "action": {
          {snip}
        },
        "inventory": {
            "maxStackSize": 1,
            "bucketTypeHash": 1498876634,
            "recoveryBucketTypeHash": 215593132,
            "tierTypeHash": 4008398120,
            "isInstanceItem": true,
            "nonTransferrableOriginal": false,
            "tierTypeName": "Legendary",
            "tierType": 5
        },
        "stats": {
          {snip}
        },
        "equippingBlock": {
          {snip}
        },
        "translationBlock": {
          {snip}
        },
        "quality": {
          {snip}
        },
        "sourceData": {
          {snip}
        },
        "acquireRewardSiteHash": 0,
        "acquireUnlockHash": 0,
        "sockets": {
          {snip}
        },
        "talentGrid": {
          {snip}
        },
        "investmentStats": [
          {snip}
        ],
        "perks": [],
        "summaryItemHash": 3520001075,
        "allowActions": true,
        "doesPostmasterPullHaveSideEffects": false,
        "nonTransferrable": false,
        "itemCategoryHashes": [
            2,
            8,
            1
        ],
        "specialItemType": 0,
        "itemType": 3,
        "itemSubType": 14,
        "classType": 3,
        "equippable": true,
        "damageTypeHashes": [],
        "damageTypes": [
            1
        ],
        "defaultDamageType": 1,
        "defaultDamageTypeHash": 3373582085,
        "hash": 1960218487,
        "index": 1707,
        "redacted": false,
        "blacklisted": false
    }

So now we know it is a Kinetic Weapon, and it is the Legendary Scout Rifle named Nameless Midnight.  These two manifest queries give us the base information about the item type and item.  In review the base item is the "stock" version.  The instanced item is the specific copy of that item associated with the player account.  The instanced item information is obtained from the API using the previously noted itemInstanceId: 6917529040022201988.


## Instanced Item Information

To get instanced item information we use the Destiny2.GetItem endpoint with the player account information (membershipType and membershipID) as well as the specific itemInstanceId.  This endpoint also uses components to determine which information is desired.

https://www.bungie.net/Platform/Destiny2/2/Profile/4611686018428389623/Item/6917529040022201988/?components=300,302,304,305


    {
        "Response": {
            "characterId": "2305843009262373961",
            "instance": {
                "data": {
                    "damageType": 1,
                    "damageTypeHash": 3373582085,
                    "primaryStat": {
                        "statHash": 1480404414,
                        "value": 305,
                        "maximumValue": 0
                    },
                    "itemLevel": 30,
                    "quality": 0,
                    "isEquipped": true,
                    "canEquip": true,
                    "equipRequiredLevel": 20,
                    "unlockHashesRequiredToEquip": [
                        2166136261
                    ],
                    "cannotEquipReason": 0
                },
                "privacy": 1
            },
            "perks": {
                "data": {
                    "perks": [
                        {
                            "perkHash": 4040885430,
                            "iconPath": "/common/destiny2_content/icons/e14a612c871f23c784e182d363f120a8.png",
                            "isActive": true,
                            "visible": true
                        },
                        {
                            "perkHash": 3114011448,
                            "iconPath": "",
                            "isActive": true,
                            "visible": false
                        },
                        {
                            "perkHash": 2666547425,
                            "iconPath": "/common/destiny2_content/icons/c2fe5ad080d1dcd3cf97674f71ecb3a6.png",
                            "isActive": true,
                            "visible": true
                        }
                    ]
                },
                "privacy": 1
            },
            "stats": {
                "data": {
                    "stats": {
                        "155624089": {
                            "statHash": 155624089,
                            "value": 49,
                            "maximumValue": 100
                        },
                        "943549884": {
                            "statHash": 943549884,
                            "value": 56,
                            "maximumValue": 100
                        },
                        "1240592695": {
                            "statHash": 1240592695,
                            "value": 51,
                            "maximumValue": 100
                        },
                        "3871231066": {
                            "statHash": 3871231066,
                            "value": 16,
                            "maximumValue": 100
                        },
                        "4043523819": {
                            "statHash": 4043523819,
                            "value": 62,
                            "maximumValue": 100
                        },
                        "4188031367": {
                            "statHash": 4188031367,
                            "value": 99,
                            "maximumValue": 100
                        },
                        "4284893193": {
                            "statHash": 4284893193,
                            "value": 180,
                            "maximumValue": 100
                        }
                    }
                },
                "privacy": 1
            },
            "sockets": {
                "data": {
                    "sockets": [
                        {
                            "plugHash": 1636108362,
                            "isEnabled": true,
                            "reusablePlugHashes": [
                                1636108362
                            ]
                        },
                        {
                            "plugHash": 2405638015,
                            "isEnabled": true,
                            "reusablePlugHashes": [
                                2405638015,
                                2405638014,
                                679077872
                            ]
                        },
                        {
                            "plugHash": 3230963543,
                            "isEnabled": true,
                            "reusablePlugHashes": [
                                3230963543,
                                3177308360
                            ]
                        },
                        {
                            "plugHash": 3038247973,
                            "isEnabled": true,
                            "reusablePlugHashes": [
                                3038247973
                            ]
                        },
                        {
                            "plugHash": 4207478320,
                            "isEnabled": true
                        },
                        {
                            "isEnabled": false
                        },
                        {
                            "plugHash": 4248210736,
                            "isEnabled": true,
                            "reusablePlugHashes": [
                                4248210736
                            ]
                        },
                        {
                            "plugHash": 236077174,
                            "isEnabled": true,
                            "reusablePlugHashes": [
                                1961001474,
                                3612467353,
                                2946649456,
                                2667900317
                            ]
                        }
                    ]
                },
                "privacy": 1
            }
        },
        "ErrorCode": 1,
        "ThrottleSeconds": 0,
        "ErrorStatus": "Success",
        "Message": "Ok",
        "MessageData": {}
    }

We provided the account for which this instanced item was associated and we were returned the characterId which is currently holding the item - and sure enough it matches the characterId that we provided when we checked the API for that character's inventory.  So the player hasn't moved this gun in the mean time.

You can see this response has general information (like damage type and the current Power Level of the item), perks, stats, and sockets.

We know this is a Kinetic weapon from the bucketHash but we can also see that the damageTypeHash: 3373582085 which if we check in the DestinyDamageTypeDefinition table of the manifest corresponds to Kinetic.  With Energy and Power weapons the player may have changed the damage type with a mod so the damageTypeHash will show the current damage type versus what "stock" damage type may be associated with the base item in the manifest.

Items like weapons and armor have a details screen in the game interface.  This screen shows a combination of the item's perks and sockets.  Sockets compose a number of different concepts like "selectable perks" (versus fixed perks), mods, shaders, ornaments, etc.  "Plugs" can be inserted into "sockets" to activate that behavior.  So in this concept a weapon may have a shader socket and the shader itself is a plug that can be used to activate the shader on that item.

An item's stats may be impacted by the specific perks and sockets used like changing the weapon sight might change range and/or handling.  An instanced item response shows the stats of the item with the specificly presented set of options configured - so these stats may differ from the stats provided in the base item (since those are the base stats with the default options).

So let's look at the two visible perks on this weapon by looking up the perkHashes in the DestinySandboxPerkDefinition table of the manifest:

    {
        "displayProperties": {
            "description": "PRECISION \nRecoil pattern is more vertical.",
            "name": "Precision Frame",
            "icon": "/common/destiny2_content/icons/e14a612c871f23c784e182d363f120a8.png",
            "hasIcon": true
        },
        "isDisplayable": true,
        "damageType": 0,
        "hash": 4040885430,
        "index": 328,
        "redacted": false,
        "blacklisted": false
    }

    {
        "displayProperties": {
            "description": "Projectiles create an area-of-effect detonation on impact.",
            "name": "Explosive Payload",
            "icon": "/common/destiny2_content/icons/c2fe5ad080d1dcd3cf97674f71ecb3a6.png",
            "hasIcon": true
        },
        "isDisplayable": true,
        "damageType": 0,
        "hash": 2666547425,
        "index": 295,
        "redacted": false,
        "blacklisted": false
    }

We can see this is a Precision Frame scout rifle with the Explosive Payload attribute.  Both of these are "fixed perks".

Then let's look at one of the sockets objects:

    {
        "plugHash": 2405638015,
        "isEnabled": true,
        "reusablePlugHashes": [
            2405638015,
            2405638014,
            679077872
        ]
    }

We can see this socket has three reusablePlugHashes so this is a "selectable perk".  We can also see which one of these three options is currently selected with plugHash (2405638015). So we lookup the plugHash in the DestinyInventoryItemDefinition table of the manifest.

    {
        "displayProperties": {
            "description": "Snapshot sight. Short zoom. \n  •  Slightly increases range\n  •  Increases handling speed",
            "name": "Red Dot 2 MOA",
            "icon": "/common/destiny2_content/icons/ce9748fa06f67f655c2f1addae333f19.png",
            "hasIcon": true
        },
        "backgroundColor": {
          {snip}
        },
        "itemTypeDisplayName": "Scope",
        "uiItemDisplayStyle": "",
        "itemTypeAndTierDisplayName": "Legendary Scope",
        "displaySource": "",
        "action": {
          {snip}
        },
        "inventory": {
          {snip}
        },
        "plug": {
          {snip}
        },
        {snip}
        "hash": 2405638015,
        "index": 2017,
        "redacted": false,
        "blacklisted": false
    }

So we can see this socket is for the sight selections on the Nameless Midnight, and the player has selected the "Red Dot 2 MOA" sight (2405638015).  While the other two plugHashes correspond to the "Red Dot Micro" (2405638014) and the "Rifle Scope SFF" (679077872).

And finally lets look at an example stats object:

    "1240592695": {
        "statHash": 1240592695,
        "value": 51,
        "maximumValue": 100
    }

We have a statHash: 1240592695 so we use the DestinyStatDefinition table of the manifest which returns:

    {
        "displayProperties": {
            "description": "Increases the effective range of this weapon.",
            "name": "Range",
            "icon": "/img/misc/missing_icon_d2.png",
            "hasIcon": false
        },
        "aggregationType": 2,
        "hasComputedBlock": false,
        "interpolate": false,
        "hash": 1240592695,
        "index": 15,
        "redacted": false,
        "blacklisted": false
    }

We can see that statHash is for Range so this instanced item of the Nameless Midnight has a range value of 51.


Again there are many different kinds of items in Destiny 2 and not all of them follow this example exactly, but this overview provides examples of many of the item concepts that will apply to the most commonly performed item queries.
