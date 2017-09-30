## Manifest Table Information (by vpzed)

Table list as of 2017/09/13:

- DestinyActivityDefinition
- DestinyActivityGraphDefinition
- DestinyActivityModeDefinition
- DestinyActivityModifierDefinition
- DestinyActivityTypeDefinition
- DestinyBondDefinition
- DestinyClassDefinition
- DestinyDamageTypeDefinition
- DestinyDestinationDefinition
- DestinyEnemyRaceDefinition
- DestinyFactionDefinition
- DestinyGenderDefinition
- DestinyHistoricalStatsDefinition
- DestinyInventoryBucketDefinition
- DestinyInventoryItemDefinition
- DestinyItemCategoryDefinition
- DestinyItemTierTypeDefinition
- DestinyLocationDefinition
- DestinyLoreDefinition
- DestinyMedalTierDefinition
- DestinyMilestoneDefinition
- DestinyObjectiveDefinition
- DestinyPlaceDefinition
- DestinyProgressionDefinition
- DestinyProgressionLevelRequirementDefinition
- DestinyRaceDefinition
- DestinyRewardSourceDefinition
- DestinySackRewardItemListDefinition
- DestinySandboxPerkDefinition
- DestinySocketCategoryDefinition
- DestinySocketTypeDefinition
- DestinyStatDefinition
- DestinyStatGroupDefinition
- DestinyTalentGridDefinition
- DestinyUnlockDefinition
- DestinyVendorCategoryDefinition
- DestinyVendorDefinition

Except for DestinyHistoricalStatsDefinition, the tables are an integer column named 'id' and a blob column named 'json'.

The DestinyHistoricalStatsDefinition table has a string column named 'key' and a blob column named 'json'.

The blob columns named 'json' contain a JSON "object/dictionary" corresponding to their respective "id".  For DestinyHistoricalStatsDefinition the 'key' corresponds to the "statId" property inside the JSON.  For other tables the 'id' corresponds to the "hash" property inside the JSON, but a transformation is required. See the [Manifest Introduction](https://github.com/vpzed/Destiny2-API-Info/wiki/Manifest-Introduction) article for more details.

For example in DestinyRaceDefinition an 'id' value -407562548 corresponds to the 'json' value of:

    {
     "displayProperties":{"description":"Human","name":"Human","hasIcon":false},
     "raceType":0,
     "genderedRaceNames":{"Male":"Human Male","Female":"Human Female"},
     "hash":3887404748,
     "index":0,
     "redacted":false
    }
