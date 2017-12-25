# Destiny 2 API Introduction - Part 2 - Account Concepts

## Introduction

In Part 1 - Setup we got our API key and setup Postman to start using the Destiny 2 API.  In this tutorial we'll continue with that foundation.

Each request we build will follow the same concept as Part 1.  We'll open a new request tab in Postman, use our Preset to add the API key header, and enter the API endpoint URL in the GET box.  Once we test that it is working, we'll save that request to the Collection.  As you work, you'll build out a Collection of all the requests you use regularly so you can see how they work and have example responses that you can use to build your application/website code around.


## Accounts 101

The Destiny 2 API uses many different kinds of accounts, account types, and groups.  Here I'll briefly summarize the most common pieces.

The first concept is membershipType and membershipId.  An account is represented by a pair of integers - the membershipType (int32) and membershipId (int64). A Bungie account is always membershipType 254.  A Destiny account has a different membershipType depending on the associated game platform with Xbox Live: 1, PlayStation Network: 2 and Battle.net: 4.  A few search related endpoints also allow the special account type of All: -1; however, most endpoints require a specific matching pair of membershipType and membershipID.

NOTE:  In Part 1 we used the special account type of -1 in our Destiny2.SearchDestinyPlayer request.  If you only want to search a specific type of membership like say Battle.net then you can use say 4 instead of -1, which will then only return matches for that platform.

So if we go back to the API Response from Part 1:

	{
		"Response": [
			{
				"iconPath": "/img/theme/destiny/icons/icon_psn.png",
				"membershipType": 2,
				"membershipId": "4611686018428389623",
				"displayName": "Dattowatto"
			}
		],
		"ErrorCode": 1,
		"ThrottleSeconds": 0,
		"ErrorStatus": "Success",
		"Message": "Ok",
		"MessageData": {}
	}

We can see that for this account the membershipType is 2 so this is a Destiny account for PSN, and the gamertag is Dattowatto as expected.  Also the unique identifier for this account is the pair of integers membershipType: 2 and membershipId: 4611686018428389623.

Each Destiny account can have up to three characters which are associated with a characterId which is also an integer (int64).

Accounts may also join Bungie.net Groups which are entities on the Bungie.net website, and players may join Clans which are a special type of Group.  Groups are identified by a groupId (int64) and a groupType (int32).  Bungie.net Groups are groupType: 0, and Clans are groupType: 1.

You will see many references throughout the documentation for membershipType, membershipId, characterId, groupType, and groupId.


## More API Requests

It is common to use a gamertag as an API entrypoint.  As before, we can use the gamertag to get the account information using the Destiny2.SearchDestinyPlayer endpoint.  We'll continue with the Datto example.  NOTE: Gamertags must be URL encoded, so say a Battle.Net user of test#1224 would become test%231234

A very frequently used endpoint is the Destiny2.GetProfile.  This endpoint uses the concept of components to return a wide array of information about a Destiny account.

Let's use the Destiny account information we retrieved for Datto to get a list of the characterIds associated with that account:

https://www.bungie.net/Platform/Destiny2/2/Profile/4611686018428389623/?components=100

	{
		"Response": {
			"profile": {
				"data": {
					"userInfo": {
						"membershipType": 2,
						"membershipId": "4611686018428389623",
						"displayName": "Dattowatto"
					},
					"dateLastPlayed": "2017-12-15T23:55:21Z",
					"versionsOwned": 3,
					"characterIds": [
						"2305843009262373961",
						"2305843009262373962",
						"2305843009286248696"
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

So now we can see that Dattowatto has three characters on this account and we have the characterIds.  Now we can use the Destiny2.GetCharacter endpoint to get additional information about a character.  In this case we'll get the character summary.  This endpoint also uses the concept of components so there are lots of different options available as described in the official documentation.

https://www.bungie.net/Platform/Destiny2/2/Profile/4611686018428389623/Character/2305843009262373961/?components=200

	{
		"Response": {
			"character": {
				"data": {
					"membershipId": "4611686018428389623",
					"membershipType": 2,
					"characterId": "2305843009262373961",
					"dateLastPlayed": "2017-12-15T23:55:21Z",
					"minutesPlayedThisSession": "161",
					"minutesPlayedTotal": "13600",
					"light": 308,
					"stats": {
						"144602215": 0,
						"392767087": 4,
						"1735777505": 0,
						"1885944937": 303,
						"1935470627": 308,
						"1943323491": 9,
						"2715839340": 20,
						"2996146975": 2,
						"3555269338": 53,
						"3897883278": 0,
						"4188031367": 40,
						"4244567218": 0
					},
					"raceHash": 898834093,
					"genderHash": 3111576190,
					"classHash": 3655393761,
					"raceType": 2,
					"classType": 0,
					"genderType": 0,
					"emblemPath": "/common/destiny2_content/icons/0b60b0d2d119a35a7883d81d9e409204.jpg",
					"emblemBackgroundPath": "/common/destiny2_content/icons/5650931ed6d9e901967e31bb9c804767.jpg",
					"emblemHash": 3115055261,
					"emblemColor": {
						"red": 2,
						"green": 3,
						"blue": 4,
						"alpha": 255
					},
					"levelProgression": {
						"progressionHash": 1716568313,
						"dailyProgress": 0,
						"dailyLimit": 0,
						"weeklyProgress": 0,
						"weeklyLimit": 0,
						"currentProgress": 196000,
						"level": 25,
						"levelCap": 25,
						"stepIndex": 25,
						"progressToNextLevel": 0,
						"nextLevelAt": 10000
					},
					"baseCharacterLevel": 25,
					"percentToNextLevel": 0
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

As you can see lots of information here and a lot of it probably doesn't make a lot of sense yet.  This is due to Bungie's desire to make the API responsive to localization.  Therefore most of the data returned is a reference which must be "de-referenced" with a localized game manifest to allow it to be presented in the respective language.  The use of the game manifest is covered in in the next article of this series.

One quick note image links in the API response have a root of https://www.bungie.net so the emblemPath above would translate to https://www.bungie.net/common/destiny2_content/icons/0b60b0d2d119a35a7883d81d9e409204.jpg


Let's say you wanted to know about which Clan Dattowatto was a member.  To look this up you'd use the GroupV2.GetGroupsForMember endpoint.  In this example we are using a filter: 0 (All) and a groupType: 1 for Clans.

https://www.bungie.net/Platform/GroupV2/User/2/4611686018428389623/0/1/


	{
		"Response": {
			"results": [
				{
					"member": {
						"memberType": 3,
						"isOnline": false,
						"groupId": "881267",
						"destinyUserInfo": {
							"iconPath": "/img/theme/destiny/icons/icon_psn.png",
							"membershipType": 2,
							"membershipId": "4611686018428389623",
							"displayName": "Dattowatto"
						},
						"joinDate": "2016-05-04T07:05:20Z"
					},
					"group": {
						"groupId": "881267",
						"name": "Math Class",
						"groupType": 1,
						"membershipIdCreated": "3952103",
						"creationDate": "2015-03-09T03:11:14.742Z",
						"modificationDate": "2017-10-01T13:41:23.811Z",
						"about": "http://math.gg",
						"tags": [],
						"memberCount": 94,
						"isPublic": true,
						"isPublicTopicAdminOnly": false,
						"primaryAlliedGroupId": "0",
						"motto": "World Class Destiny Clan. English only, please.",
						"allowChat": true,
						"isDefaultPostPublic": false,
						"chatSecurity": 0,
						"locale": "en",
						"avatarImageIndex": 70025,
						"homepage": 1,
						"membershipOption": 2,
						"defaultPublicity": 2,
						"theme": "Group_Community1",
						"bannerPath": "/img/Themes/Group_Community1/struct_images/group_top_banner.jpg",
						"avatarPath": "/img/profile/avatars/group/025.png",
						"isAllianceOwner": false,
						"conversationId": "9537586",
						"enableInvitationMessagingForAdmins": false,
						"banExpireDate": "2001-01-01T00:00:00Z",
						"features": {
							"maximumMembers": 100,
							"maximumMembershipsOfGroupType": 1,
							"capabilities": 31,
							"membershipTypes": [
								1,
								2,
								4,
								10
							],
							"invitePermissionOverride": true,
							"updateCulturePermissionOverride": true,
							"hostGuidedGamePermissionOverride": 2,
							"updateBannerPermissionOverride": true,
							"joinLevel": 2
						},
						"clanInfo": {
							"d2ClanProgressions": {
								"584850370": {
									"progressionHash": 584850370,
									"dailyProgress": 300000,
									"dailyLimit": 0,
									"weeklyProgress": 100000,
									"weeklyLimit": 100000,
									"currentProgress": 300000,
									"level": 3,
									"levelCap": 6,
									"stepIndex": 3,
									"progressToNextLevel": 75000,
									"nextLevelAt": 125000
								},
								"1273404180": {
									"progressionHash": 1273404180,
									"dailyProgress": 0,
									"dailyLimit": 0,
									"weeklyProgress": 0,
									"weeklyLimit": 0,
									"currentProgress": 0,
									"level": 1,
									"levelCap": 6,
									"stepIndex": 1,
									"progressToNextLevel": 0,
									"nextLevelAt": 1
								},
								"3381682691": {
									"progressionHash": 3381682691,
									"dailyProgress": 0,
									"dailyLimit": 0,
									"weeklyProgress": 0,
									"weeklyLimit": 0,
									"currentProgress": 0,
									"level": 1,
									"levelCap": 6,
									"stepIndex": 1,
									"progressToNextLevel": 0,
									"nextLevelAt": 1
								},
								"3759191272": {
									"progressionHash": 3759191272,
									"dailyProgress": 0,
									"dailyLimit": 0,
									"weeklyProgress": 0,
									"weeklyLimit": 0,
									"currentProgress": 0,
									"level": 1,
									"levelCap": 6,
									"stepIndex": 1,
									"progressToNextLevel": 0,
									"nextLevelAt": 1
								}
							},
							"clanCallsign": "MATH",
							"clanBannerData": {
								"decalId": 4142223390,
								"decalColorId": 3312277351,
								"decalBackgroundColorId": 3501638311,
								"gonfalonId": 1473910866,
								"gonfalonColorId": 2157636321,
								"gonfalonDetailId": 1664476157,
								"gonfalonDetailColorId": 4095345228
							}
						}
					}
				}
			],
			"totalResults": 1,
			"hasMore": false,
			"query": {
				"itemsPerPage": 1000,
				"currentPage": 1
			},
			"useTotalResults": true
		},
		"ErrorCode": 1,
		"ThrottleSeconds": 0,
		"ErrorStatus": "Success",
		"Message": "Ok",
		"MessageData": {}
	}
	
Again lots of information here, but the key information is yes Dattowatto is in a Clan with groupId: 881267, and name: "Math Class".

Hopefully this helps introduce the concepts of accounts and groups in Destiny 2.  In Part 3 we'll get started with some introductory information on the game manifest.

