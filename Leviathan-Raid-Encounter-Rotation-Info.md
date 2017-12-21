## Leviathan Raid Encounter Rotation Info

NOTE:  The Leviathan Raid Encounter Rotation information was provided by **Wayno156** on the Destiny API Discussion Discord server.  This article by vpzed is an explanation around the information **he provided**.

The Leviathan Raid has two modes (Normal and Prestige) and the intermediary encounter order rotates each week.  At the time of this writing the Power Level of the Normal version is 300 and the Prestige version is 330, so another way to look at the table below is Normal and Prestige since Power Levels change over time.

By using the table below one can determine the weekly Leviathan raid encounter order from the activityHash information in the Milestone endpoint output (or other activityHash references).

For example from this week's Milestone API response:

    "3660836525": {
         "milestoneHash": 3660836525,
         "availableQuests": [
            {
                "questItemHash": 82550967,
                "activity": {
                    "activityHash": 2693136604,
                    "variants": [
                        {
                            "activityHash": 2693136604,
                            "activityModeHash": 2043403989,
                            "activityModeType": 4
                        },
                        {
                            "activityHash": 757116822,
                            "activityModeHash": 2043403989,
                            "activityModeType": 4
                        }

We can see the activityHashes for the Leviathan Raid (activityModeType: 4 is Raid) are 2693136604 and 757116822.

So from the table below we can see that the encounter order is Gauntlet, Royal Pools, Pleasure Gardens.


    hash	       power	detail
    2693136605	300	Gauntlet, Pleasure Gardens, Royal Pools
    2693136604	300	Gauntlet, Royal Pools, Pleasure Gardens
    2693136602	300	Pleasure Gardens, Gauntlet, Royal Pools
    2693136603	300	Pleasure Gardens, Royal Pools, Gauntlet
    2693136600	300	Royal Pools, Gauntlet, Pleasure Gardens
    2693136601	300	Royal Pools, Pleasure Gardens, Gauntlet
    1685065161	330	Gauntlet, Pleasure Gardens, Royal Pools
    757116822	330	Gauntlet, Royal Pools, Pleasure Gardens
    417231112	330	Pleasure Gardens, Gauntlet, Royal Pools
    3446541099	330	Pleasure Gardens, Royal Pools, Gauntlet
    2449714930	330	Royal Pools, Gauntlet, Pleasure Gardens
    3879860661	330	Royal Pools, Pleasure Gardens, Gauntlet


NOTE:  To be clear this raid encounter order information was compiled by Wayno156, and this information is not from the Bungie API directly.  To use this in your application you would create a lookup method in your application to do the de-referencing.
