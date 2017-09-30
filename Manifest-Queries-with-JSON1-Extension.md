## SQLite Queries using the JSON1 extension (by vpzed)

NOTE:  These require a SQLite query program with the JSON1 extensions.  These work fine in "DB Browser for SQLite" for example.

As of the 2017/09/13 Manifest all tables use the numeric "hash" property as a unique identifier inside the 'json' column, 
except DestinyHistoricalStatsDefinition which uses the string "statId" property.

Using the JSON1 extension you can bypass using the 'id' or 'key' columns of the manifest and query the 'json' columns of the tables directly.  This means you can query directly using what is received from the API response instead of having to transform it like with "hash" to 'id'.

Returns all properties on the matched row and use an exact match on the hash:

    SELECT json_extract(DestinyRaceDefinition.json, '$')
    FROM DestinyRaceDefinition, json_tree(DestinyRaceDefinition.json, '$')
    WHERE json_tree.key = 'hash' AND json_tree.value = 2803282938

Return only the displayProperties.name property on the row and use an exact match on the hash:

    SELECT json_extract(DestinyRaceDefinition.json, '$.displayProperties.name')
    FROM DestinyRaceDefinition, json_tree(DestinyRaceDefinition.json, '$')
    WHERE json_tree.key = 'hash' AND json_tree.value = 2803282938

Case insensitive search for a "keyword" in the name property:

    SELECT json_extract(DestinyActivityModeDefinition.json, '$')
    FROM DestinyActivityModeDefinition, json_tree(DestinyActivityModeDefinition.json, '$')
    WHERE json_tree.key = 'name' AND UPPER(json_tree.value) LIKE UPPER('%strike%')

Return rows that match a list of values:

    SELECT json_extract(DestinyInventoryItemDefinition.json, '$')
    FROM DestinyInventoryItemDefinition, json_tree(DestinyInventoryItemDefinition.json, '$')
    WHERE json_tree.key = 'itemType' AND json_tree.value IN (2,3,4)

Return rows that match a Boolean value of "true":

    SELECT json_extract(DestinyActivityDefinition.json, '$.hash') as hash
    FROM DestinyActivityDefinition, json_tree(DestinyActivityDefinition.json)
    WHERE json_tree.key = 'redacted' AND json_tree.value
