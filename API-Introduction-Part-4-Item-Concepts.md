# Destiny 2 API Introduction - Part 4 - Item Concepts

## Introduction

In Parts 1-3 we introduced the Destiny 2 API and the game manifest.  Another key concept in the API is items.  The API treats items in an abstract fashion so an item doesn't just mean a normal in-game character item like a helmet or shotgun.  In this article we'll go over some of the general item concepts.


## Base items and Instanced Items

First off there is the concept of the base item and an instanced item.  The base item is a static representation of the item in the game and it is defined by the manifest entry for that item.  This is the "stock" version of the item and defines the base stats of the item with the default perks selected (if applicable), default shader (if applicable), etc.  Then there is an instanced item.  This is the specific copy of the base item associated with a particular account - with the player chosen perks selected, shader, mods, etc. applied.  Instanced item details are provided by the API, and while the information about an instanced items may still reference the manifest - a view of an instanced item from the API is a point-in-time view of the specific item associated with that player account.  One second after you get your API response the player could apply a new shader and your point-in-time view of the item would then be out of date, for example.


## General Items

The general type of items are the ones shown in the character overview screen:

Kinetic weapons  
Energy weapons  
Power weapons  
Ghost shells  
Helmets  
Gauntlets  
Chest armor  
Leg armor  
Class items  
Sparrows  
Ships  
Emblems  
Emotes  

Kinetic and Energy weapons include Auto Rifle, Hand Cannon, Pulse Rifle, Scout Rifle, Sidearm, Submachine gun, and Trace Rifle.

Power weapons include Fusion rifle, Grenade launcher, Linear fusion rifle, Rocket launcher, Sniper Rifle, and Sword.

Item tiers include Basic (white), Common (green), Rare (blue), Legendary (purple), and Exotic (yellow).

Different items may have progression requirements like a minimum required level to equip.  Players may receive items in-game when they don't meet a given item's requirements, but they won't be able to equip them until the requirements are met.


## Other items

There are lots of other items in Destiny 2.  Again the general types are shown in the character overview screen:

Engrams    
Consumables  
Mods (including Ornaments)  
Shaders  

Each of these item types consist of dozens of different items.  Engrams holds found engrams of the various tiers that require decryption by a Cryptarch.  Consumables has planetary materials, Gunsmith parts, reputation tokens, etc.  Mods consist of weapon and armor mods, weapon and armor ornaments, trasmat effects, etc.  Shaders work essentially like a mod and change the coloration of the item.

There are also Auras which are status effects applied to a character for completing a certain activity like the Prestige Nightfall, and Clan Banners which are given to players who choose to join a clan.


## Item Buckets

As you can see in the player interface different items are displayed in the user interface in different sections.  For example the on-character Kinetic weapon section of the character screen.  That Kinetic weapon section is called a bucket.  Items are stored in various buckets of different sizes, for example the on-character Kinetic weapon section allows one equipped item and nine un-equipped items.  Sometimes these buckets are used to hold special items like Quests, for example the Mida Multi-Tool exotic scout rifle quest will be stored in the Kinetic weapon bucket.  The Vault and Postmaster are other examples of buckets.


## Base items and the Manifest

Most base items are defined in the game manifest in the DestinyInventoryItemDefinition table.  Item tiers are defined in the DestinyItemTierTypeDefinition table.  Item damage types are defined in the DestinyDamageTypeDefinition table.  Item stats are defined in the DestinyStatDefinition table.  Item buckets are defined in the DestinyInventoryBucketDefinition.


## Items you might not think are items

The Destiny 2 API treats many things as items which you might not think are items.  Subclass abilities, perks, weapon sights, missions, and adventures are a few.  If you are wondering where the definition of something is in the manifest and there doesn't appear to be a clear reference like classHash pointing to DestinyClassDefinition, then DestinyInventoryItemDefinition is often a good place to start.


This article covered some introductory item concepts.  In the next article we'll use the API to exercise some of these concepts.