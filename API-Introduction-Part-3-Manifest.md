# Destiny 2 API Introduction - Part 3 - Manifest

## Introduction

The Destiny game manifest is a SQLite database that comes in several languages with localized versions of the game information specific to that language.  Most of the API responses utilize references (often {something}Hash) to point the user to the manifest.


## Retrieving the Manifest Database

The endpoint URL to retrieve the base information about the manifest is:

https://www.bungie.net/Platform/Destiny2/Manifest/

	{
		"Response": {
			"version": "61252.17.12.02.0200-8",
			"mobileAssetContentPath": "/common/destiny2_content/sqlite/asset/asset_sql_content_8bd670a833634c5c92de2c96ba514f3e.content",
			"mobileGearAssetDataBases": [
				{
					"version": 0,
					"path": "/common/destiny2_content/sqlite/asset/asset_sql_content_8bd670a833634c5c92de2c96ba514f3e.content"
				},
				{
					"version": 1,
					"path": "/common/destiny2_content/sqlite/asset/asset_sql_content_d604e05133733779297d1548f0ee208c.content"
				},
				{
					"version": 2,
					"path": "/common/destiny2_content/sqlite/asset/asset_sql_content_d604e05133733779297d1548f0ee208c.content"
				}
			],
			"mobileWorldContentPaths": {
				"en": "/common/destiny2_content/sqlite/en/world_sql_content_8ce5b3356e8749ddcb7f81c0ac8875c6.content",
				"fr": "/common/destiny2_content/sqlite/fr/world_sql_content_4100c5c985b2488c44a6ebee97037677.content",
				"es": "/common/destiny2_content/sqlite/es/world_sql_content_9bfbf7737c5982d9b0bea1ce98e8e898.content",
				"de": "/common/destiny2_content/sqlite/de/world_sql_content_2bacc28fa685ab4c2c49257a46742668.content",
				"it": "/common/destiny2_content/sqlite/it/world_sql_content_d8b5fc81eebec797ba66f1e0868361c7.content",
				"ja": "/common/destiny2_content/sqlite/ja/world_sql_content_ea68ca09f36431845bbafa8e273e8ad9.content",
				"pt-br": "/common/destiny2_content/sqlite/pt-br/world_sql_content_6bae2d1604335fa58328c45e935c5d25.content",
				"es-mx": "/common/destiny2_content/sqlite/es-mx/world_sql_content_85f7afcf28b78176f669d2d435afa18f.content",
				"ru": "/common/destiny2_content/sqlite/ru/world_sql_content_45cddf538cfe61d81cc8d54f800400ac.content",
				"pl": "/common/destiny2_content/sqlite/pl/world_sql_content_b11a2fd690037964075c3e51224485b9.content",
				"zh-cht": "/common/destiny2_content/sqlite/zh-cht/world_sql_content_33e179d09ec3c09c05792906feaec03c.content"
			},
			"mobileClanBannerDatabasePath": "/common/destiny2_content/clanbanner/clanbanner_sql_content_13bcd2427f07d0121bb532a1a0638ba4.content",
			"mobileGearCDN": {
				"Geometry": "/common/destiny2_content/geometry/platform/mobile/geometry",
				"Texture": "/common/destiny2_content/geometry/platform/mobile/textures",
				"PlateRegion": "/common/destiny2_content/geometry/platform/mobile/plated_textures",
				"Gear": "/common/destiny2_content/geometry/gear",
				"Shader": "/common/destiny2_content/geometry/platform/mobile/shaders"
			}
		},
		"ErrorCode": 1,
		"ThrottleSeconds": 0,
		"ErrorStatus": "Success",
		"Message": "Ok",
		"MessageData": {}
}


The main game world database is listed in the Response.mobileWorldContentPaths object for your desired language.  I use English so the one I download is Response.mobileWorldContentPaths.en  The other manifest databases like the mobileClanBannerDatabasePath are for very specific use cases and will not be covered in this tutorial.

The base URL is https://www.bungie.net so for for the Response.mobileWorldContentPaths.en above that would give:

https://www.bungie.net/common/destiny2_content/sqlite/en/world_sql_content_8ce5b3356e8749ddcb7f81c0ac8875c6.content

Hitting that URL will download a file with the same name so in this case world_sql_content_8ce5b3356e8749ddcb7f81c0ac8875c6.content

Even though it doesn't indicate it with a file extension that is a ZIP file, so you need to unzip it. Since the file inside the zip has the same name I rename it to world_sql_content_8ce5b3356e8749ddcb7f81c0ac8875c6.zip and then extract the archive.  As mentioned the unzipped file will be world_sql_content_8ce5b3356e8749ddcb7f81c0ac8875c6.content again.  This is the SQLite database file so I rename this to world_sql_content_8ce5b3356e8749ddcb7f81c0ac8875c6.sqlite3 for easy recognition on my PC.  In your code these renaming steps are not required.

NOTE: The string "8ce5b3356e8749ddcb7f81c0ac8875c6" is the md5sum of the **unzipped** manifest file, so you can use the file name to verify the checksum and know your file came through the process OK.

Then you use a SQLite3 program to interact with that unzipped file.  For this tutorial I will use [DB Browser for SQLite](http://sqlitebrowser.org/) and the [SQLite docs](https://sqlite.org/).

All but one of the tables have an 'id' column and 'json' column.  The 'json' column contains a JSON object with the actual data inside which is a lot like working with the JSON responses from the API.  One exception is the DestinyHistoricalStatsDefinition table which uses a 'key' and 'json' setup.  The table names use a format of Destiny{identifier}Definition so DestinyRaceDefinition is the table with Race information.


## Manifest Updates

The manifest updates on an irregular schedule.  If your application relies heavily on the manifest you may want to check for updates every day. A simple method for checking for a new manifest is to compare the URL of the desired manifest file (i.e. Response.mobileWorldContentPaths.en for English) between retrievals.  If the name has changed then the manifest has updated.


## Manifest Lookups

The most common use case for the manifest is the API is going to give you something like say raceHash = 2803282938 but what you want is the name of that race so you need to look that up in the manifest.

In all programming languages, there is a limit to the number of digits an integer datatype variable can hold. In the Destiny Manifest, apart from a couple of items, majority of them have more than 10 digits in them, which cannot be held in integer variables, thus preventing you from performing queries using them. The below code snippet is used to convert such large numbers into a negative number within the integer datatype limits.

Convert that hash into a manifest id column value with something like this Python snippet:

    val = int(val)
    if (val & (1 << (32 - 1))) != 0:
        val = val - (1 << 32)
		
For a raceHash of 2803282938 that will give you an id of -1491684358.  

Then use that id in a SQL query like:

    SELECT json FROM DestinyRaceDefinition WHERE id = -1491684358;


NOTE:  For some additional code examples see the DestinyDevs Wiki [here](http://destinydevs.github.io/BungieNetPlatform/docs/Manifest). This is for the D1 manifest but the same concepts apply.

Another way to query the manifest is the use the SQLite JSON extensions.  This allows you to query the json column directly and bypass the use of the id column and the conversions it entails.

To perform the same query with JSON extensions you would use a SQL query like:

	SELECT json_extract(DestinyRaceDefinition.json, '$')
	FROM DestinyRaceDefinition, json_tree(DestinyRaceDefinition.json, '$')
	WHERE json_tree.key = 'hash' AND json_tree.value = 2803282938

This would return:

	{
		"displayProperties": {
			"description": "The Awoken survived the Collapse in deep space. But they were forever changed.",
			"name": "Awoken",
			"hasIcon": false
		},
		"raceType": 1,
		"genderedRaceNames": {
			"Male": "Awoken Male",
			"Female": "Awoken Female"
		},
		"hash": 2803282938,
		"index": 1,
		"redacted": false,
		"blacklisted": false
	}

So you can see the displayProperties.name value is Awoken.

A separate article on this Wiki includes many example queries for the use of JSON extensions.


## Another Example

In Part 2 we looked-up the information for one of Dattowatto's characters and that returned "classHash": 3655393761.  Some of the references are clear like this one in that classHash points us to the DestinyClassDefinition table.

Let's say all we wanted was the class name.  To accomplish that we'd use this SQL query:

	SELECT json_extract(DestinyClassDefinition.json, '$.displayProperties.name')
	FROM DestinyClassDefinition, json_tree(DestinyClassDefinition.json, '$')
	WHERE json_tree.key = 'hash' AND json_tree.value = 3655393761

which returns Titan.

or using the id field 3655393761 converts to -639573535 so we'd use:

	SELECT json FROM DestinyClassDefinition WHERE id = -639573535;
	
which returns:

	{
		"classType": 0,
		"displayProperties": {
			"name": "Titan",
			"hasIcon": false
		},
		"genderedClassNames": {
			"Male": "Titan",
			"Female": "Titan"
		},
		"hash": 3655393761,
		"index": 0,
		"redacted": false,
		"blacklisted": false
	}


## Manifest Tables

As of Dec 2017 there are 39 manifest tables detailing game information.

DestinyActivityDefinition  
DestinyActivityGraphDefinition  
DestinyActivityModeDefinition  
DestinyActivityModifierDefinition  
DestinyActivityTypeDefinition  
DestinyBondDefinition  
DestinyClassDefinition  
DestinyDamageTypeDefinition  
DestinyDestinationDefinition  
DestinyEnemyRaceDefinition  
DestinyEquipmentSlotDefinition  
DestinyFactionDefinition  
DestinyGenderDefinition  
DestinyHistoricalStatsDefinition  
DestinyInventoryBucketDefinition  
DestinyInventoryItemDefinition  
DestinyItemCategoryDefinition  
DestinyItemTierTypeDefinition  
DestinyLocationDefinition  
DestinyLoreDefinition  
DestinyMedalTierDefinition  
DestinyMilestoneDefinition  
DestinyObjectiveDefinition  
DestinyPlaceDefinition  
DestinyProgressionDefinition  
DestinyProgressionLevelRequirementDefinition  
DestinyRaceDefinition  
DestinyReportReasonCategoryDefinition  
DestinyRewardSourceDefinition  
DestinySackRewardItemListDefinition  
DestinySandboxPerkDefinition  
DestinySocketCategoryDefinition  
DestinySocketTypeDefinition  
DestinyStatDefinition  
DestinyStatGroupDefinition  
DestinyTalentGridDefinition  
DestinyUnlockDefinition  
DestinyVendorCategoryDefinition  
DestinyVendorDefinition  


## Unofficial Manifest Resources

There are a number of unofficial game manifest resources.  Here are a couple that provide different ways to get manifest data:

https://destiny.plumbing/  provides processed manifest data in JSON format for Destiny 2.

https://destinysets.com/data  provides a visual manifest interface for Destiny 2.
