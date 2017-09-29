## Destiny 2 Manifest Endpoint Introduction

The endpoint URL to retrieve the base information about the manifest is:

https://www.bungie.net/Platform/Destiny2/Manifest/

The endpoint will return an API response like this:

	{
		"Response": {
			"version": "59912.17.09.13.1822-11",
			"mobileAssetContentPath": "/common/destiny2_content/sqlite/asset/asset_sql_content_30228cabcdcf681095c36fab1da69e32.content",
			"mobileGearAssetDataBases": [
				{
					"version": 0,
					"path": "/common/destiny2_content/sqlite/asset/asset_sql_content_30228cabcdcf681095c36fab1da69e32.content"
				},
				{
					"version": 1,
					"path": "/common/destiny2_content/sqlite/asset/asset_sql_content_669cef97d6bb31f46d5703bcb9acb7bd.content"
				},
				{
					"version": 2,
					"path": "/common/destiny2_content/sqlite/asset/asset_sql_content_669cef97d6bb31f46d5703bcb9acb7bd.content"
				}
			],
			"mobileWorldContentPaths": {
				"en": "/common/destiny2_content/sqlite/en/world_sql_content_4a0f279aaf9e1914bfcd210a1edea4a4.content",
				"fr": "/common/destiny2_content/sqlite/fr/world_sql_content_e16a0e9ad5b76bba5cba1fb9c5ab7132.content",
				"es": "/common/destiny2_content/sqlite/es/world_sql_content_020834e812d6f3a19fccd95362ce2f45.content",
				"de": "/common/destiny2_content/sqlite/de/world_sql_content_4158695949f759d14f2c0aee337749be.content",
				"it": "/common/destiny2_content/sqlite/it/world_sql_content_2117377067d2fec0091179bf7ef3b608.content",
				"ja": "/common/destiny2_content/sqlite/ja/world_sql_content_bd81cfd607c868368fece4f328fe41f4.content",
				"pt-br": "/common/destiny2_content/sqlite/pt-br/world_sql_content_a33a5dd697c963cf1f35b4f9a49d880c.content",
				"es-mx": "/common/destiny2_content/sqlite/es-mx/world_sql_content_8df03f0f54db721306bd7bd1ab1b35d1.content",
				"ru": "/common/destiny2_content/sqlite/ru/world_sql_content_4848f7a36634ca656e30d957dbad29a8.content",
				"pl": "/common/destiny2_content/sqlite/pl/world_sql_content_24a12108e96ab07bd9910a1b6b08709e.content",
				"zh-cht": "/common/destiny2_content/sqlite/zh-cht/world_sql_content_0b0ea27db850ea7e5d357cffd70b1eb3.content"
			},
			"mobileClanBannerDatabasePath": "/common/destiny2_content/clanbanner/clanbanner_sql_content_12a291b583d3f5ce5c30a648fa58b482.content",
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


Most people will be interested in the mobileWorldContentPaths item for your desired language.  I use English so the one I download is Response.mobileWorldContentPaths.en  The other manifest .content files are for very specific use cases so most folks don't use them.

The base URL is http://www.bungie.net so for for the Response.mobileWorldContentPaths.en above that would give:

    http://www.bungie.net/common/destiny2_content/sqlite/en/world_sql_content_4a0f279aaf9e1914bfcd210a1edea4a4.content

Hitting that URL will download a file with the same name so in this case world_sql_content_4a0f279aaf9e1914bfcd210a1edea4a4.content

Even though it doesn't indicate it with a file extension that is a zip file, so you need to unzip it. Since the file inside the zip has the same name I rename it to world_sql_content_4a0f279aaf9e1914bfcd210a1edea4a4.zip.  As mentioned the unzipped file will be world_sql_content_4a0f279aaf9e1914bfcd210a1edea4a4.content again.  This is the SQLite database file so I rename this to world_sql_content_4a0f279aaf9e1914bfcd210a1edea4a4.sqlite

Then you use a SQLite3 program to interact with that unzipped file.  To just open it and look at the data to mess around I use [DB Browser for SQLite](http://sqlitebrowser.org/) and the [SQLite docs](https://sqlite.org/).

All the tables are basically some sort of id column and json column.  The json column contains a JSON object with the actual data inside which is a lot like working with the JSON responses from the API.

The most common use case for the manifest is the API is going to give you something like say raceHash = 2803282938 but what you want is the name of that race so you need to look that up in the manifest.

So you convert that hash into a manifest id column value with something like this Python snippet:

    val = int(val)
    if (val & (1 << (32 - 1))) != 0:
        val = val - (1 << 32)
		
For a raceHash of 2803282938 that will give you an id of -1491684358.  

Then use that id in a SQL query like:

    SELECT json FROM DestinyRaceDefinition WHERE id = -1491684358;


For some additional code examples see the DestinyDevs Wiki [here](http://destinydevs.github.io/BungieNetPlatform/docs/Manifest). This is for the D1 manifest but the same concepts apply.