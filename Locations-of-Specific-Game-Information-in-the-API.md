## Locations of Specific Game Information in the API (by kbreit)

Table list as of 2017/10/12:

* Nightfall
* Flashpoint
* Iron Banner

### Nightfall
Endpoint: https://www.bungie.net/Platform/Destiny2/Milestones/

JSON Path: Response.2171429505.availableQuests.0.activity

Components:
* Event name
* Modifiers
* Challenges
* Beginning and ending dates

### Flashpoint
Endpoint: https://www.bungie.net/Platform/Destiny2/Milestones/

JSON Path: Response.463010297.availableQuests

Components:
* Location
* Beginning and ending dates

### Iron Banner
Endpoint: https://www.bungie.net/Platform/Destiny2/Milestones/

JSON Path: Response.4248276869.milestoneHash

Components:
* Milestone goal (ex. Complete 10 Iron Banner ranks)
* Daily objectives