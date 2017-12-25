# Destiny 2 API Introduction - Part 1 - Setup

## Introduction

This tutorial series will go over some beginner information on how to use the Destiny 2 API. This section will go over the initial setup needed to use the API for testing public API endpoint requests and responses.


## Bungie.net API Key

This article assumes that you already have a Bungie.net account.  If you do not already have one then you can create one here https://www.bungie.net/en/User/JoinUp

When logged into your Bungie.net account you can access the Application page to request an API key here https://www.bungie.net/en/Application

Select the Create New App button to enter the New Application screen. You can create multiple applications with different configurations. For this initial introduction we will not be using OAuth authentication.  This topic will be covered in a future article.

In the New Application screen enter your Application Name and Website.  In the App Authentication > OAuth Client Type, select Not applicable.  Leave the Redirect URL blank.  Under Scope select, "Read your Destiny vault and character inventory" and "Read your Destiny vendor and advisor information".  Under Browser Based Apps > Origin Header enter *   Check the box to agree to the Terms of Use, and click the Create New App button.

Now in the API Keys section you should see your API key.  You will use this key in every Bungie API request you make.  This key is private and should not be shared, published in your source code, etc.  Copy the API Key into a safe place as you will use it later in this tutorial.


## Destiny 2 API Documentation

The home of Destiny 2 API information is https://github.com/Bungie-net/api and the main API documentation like is https://bungie-net.github.io/multi/index.html so bookmark these links as you will refer to them frequently.  It is recommended that you create a GitHub account as this allows you to post or follow Issues for the repository.  


## API Tool

To perform your initial interactions with the API the use of an API tool is highly recommended. The Postman tool is free and is what we'll use in this tutorial.  Postman can be downloaded from https://www.getpostman.com/ for macOS, Windows, and Linux.

Once Postman is installed, use the Github account you created to sign-in to Postman.  This will save your setup and sync it across installations.

In Postman click Collection > New Collection and name it Destiny or something you want to use for your project.  A Collection is a group of API request setups.  You can then save different API endpoints so you don't have to re-enter the information each time.

In the middle panel you should see an API request tab.  There is a URL input box next to the request type which defaults to GET.  Just under that box there is a row of options: Authorization, Headers, Body, Pre-request Script, Tests.  Select the Headers option.

On the right-hand side of the window in the Headers screen click Presets > Manage Presets.  In the pop-up window click Add.  Enter a preset name and then in the boxes below enter:

Key: X-API-KEY  
Value: {paste your API key here}  
Description: Destiny  

Save and close the Manage Presets screen.  You should now see your preset in the Preset menu.  Select your preset to add your API key to the request.  It should show up in the Headers with a checkbox on the left.  You will need to use your prest to add the API key header to each request you build in Postman.


## First API Request

Now on to your first API request!  We'll use the popular Destiny streamer Datto to do an example request.  We'll explain more about API requests in the next section of the tutorial, but for now enter this in the box next to GET

https://www.bungie.net/Platform/Destiny2/SearchDestinyPlayer/-1/dattowatto/

Click the Send button.  If you entered your API key correctly you should see this response (or similar):

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

Click the Save button, enter Destiny2.SearchDestinyPlayer, select your Destiny collection, and click Save to save your new request to the Collection.

This completes Part 1 - Setup.  In Part 2 we'll go over common entry points for the API and how to perform common requests.


