#Smartthings Python API Wrapper

Simple python wrapper for smartthigns. Should work to access devices and is most likely better than screen scraping.

I did most of this work when i was monitoring our vacant place for temperature to ensure that the pipes didn't freeze. I extracted the codes and here yea go. You should be able to do some interesting things with this endpoint/data.

A lot of the info i got from [SmartThings Unofficial Documentation](https://github.com/tmaiaroto/smartthings-unofficial-docs/blob/master/Documentation.md). I also copied some of the docs from there to here.

This should be accurate, but there may be a few details that don't exactly work

##Get started

	from smartthings import smartthings

	if __name__ == "__main__":
    	username = "user@gmail.com"
	    password = "longverysecurepassword"
    	api = smartthings(username=username, password=password)
	    print api.accounts()


    	print api.events(account_id, max=1):


Pretty simple!

##Endpoints:


###Accounts

Get information about the accounts for the currently authenticated user.

   `api.accounts()`

   Example response:

	[
		{
			id: "<idHashString>",
			name: "<email>'s Account",
			fullName: "<yourName>",
			locked: false,
			permissions: "a"
		}
	]

###Locations

Get information about locations for a specific account.

    account_id = "xxx"
    api.locations(account_id)

###Events
Get the recent events for an account.

    api.events(account_id, max=1):

**Params**
`all=<boolean>`
`max=<number>`
`source=<boolean>`

The default is to display the ten most recent displayed events. Changing this will number will return more or less events. For example: https://graph.api.smartthings.com/api/accounts/idHashString/events?max=20

If `all` is true, then all events will be returned. If `source` is true, then only events not displayed back to the user will be returned. This would include some more detailed information from the sensors including if they are battery powered or not, etc.

Example response:

	[
		{
			id: "<uuid>",
			hubId: "<idHashString>",
			description: "SmartSense Motion detected motion has stopped",
			displayed: true,
			linkText: "SmartSense Motion",
			date: "2013-05-22T23:20:04.126Z",
			unixTime: 1369264804126,
			value: "inactive",
			deviceId: "<idHashString>",
			name: "motion",
			locationId: "<idHashString>"
		},
		{
			id: "<uuid>",
			hubId: "<idHashString>",
			description: "SmartSense Motion detected motion",
			displayed: true,
			linkText: "SmartSense Motion",
			date: "2013-05-22T23:19:02.291Z",
			unixTime: 1369264742291,
			value: "active",
			deviceId: "<idHashString>",
			name: "motion",
			locationId: "<idHashString>"
		},
		...
	]


###Hubs

Get information about the connected hubs.

    print api.hubs()

Example response:

	[
		{
			id: "<idHashString>",
			name: "My House",
			locationId: "<idHashString>",
			firmwareVersion: "000.009.00004",
			zigbeeId: "<idString>",
			status: "ACTIVE",
			onlineSince: "2013-05-22T22:00:44.858Z",
			signalStrength: null,
			batteryLevel: null,
			type: {
				name: "Hub"
			},
			virtual: false,
			permissions: "a"
		}
	]



###Hub
Get information about a hub, the connected devices, and recent events.

    print api.hub(hub_id)

Example response:

	{
		account: {
			id: "<idHashString>",
			name: "<email>'s Account",
			fullName: "<yourName>",
			locked: false,
			permissions: "a"
		},
		locations: [
			{
				id: "<idHashString>",
				name: "Old apt",
				accountId: "<idHashString>",
				latitude: "",
				longitude: "",
				regionRadius: 50,
				backgroundImage: "https://smartthings-location-images.s3.amazonaws.com/standard/standard51.jpg",
				mode: {
					id: "<idHashString>",
					name: "Home"
				},
				modes: [
					{
						id: "<idHashString>",
						name: "Away"
					},
					{
						id: "<idHashString>",
						name: "Home"
					},
					{
						id: "<idHashString>",
						name: "Night"
					}
				],
				permissions: "a"
			}
		],
		events: [
			{
				id: "<uuid>",
				hubId: "<idHashString>",
				description: "SmartSense Motion detected motion has stopped",
				displayed: true,
				linkText: "SmartSense Motion",
				date: "2013-05-23T00:11:10.900Z",
				unixTime: 1369267870900,
				value: "inactive",
				deviceId: "<idHashString>",
				name: "motion",
				locationId: "<idHashString>"
			},
			…
		]
	}

###Hub Events
Get the last few (currently 10) events for a hub.

    #print api.hub_events(hub_id)


**Params**
`all=<boolean>`
`max=<number>`
`source=<boolean>

###Hub Devices

Get the devices paired with a hub (also available in the hub details response).

    print api.hub_devices(hub_id)

Example response:

	[
		{
			id: "<idHashString>",
			name: "SmartSense Motion",
			hubId: "<idHashString>",
			label: null,
			status: "ACTIVE",
			currentStates: [
				{
					name: "motion",
					value: "inactive",
					unit: null,
					date: "2013-05-23T05:57:41Z",
					unixTime: 1369288661864
				}
			],
			typeId: "<uuid>",
			deviceNetworkId: "<id>",
			virtual: false,
			primaryTileName: null,
			stateOverrides: [ ],
			permissions: "a"
		},
		{
			id: "<idHashString>",
			name: "SmartSense Multi",
			hubId: "<idHashString>",
			label: null,
			status: "ACTIVE",
			currentStates: [
			{
				name: "threeAxis",
				value: "46,12,1016",
				unit: null,
				date: "2013-05-23T05:49:47Z",
				unixTime: 1369288187171
			},
			{
				name: "temperature",
				value: "65",
				unit: "F",
				date: "2013-05-23T05:54:50Z",
				unixTime: 1369288490264
			},
			{
				name: "acceleration",
				value: "inactive",
				unit: null,
				date: "2013-05-23T05:54:50Z",
				unixTime: 1369288490264
			},
			{
				name: "contact",
				value: "closed",
				unit: null,
				date: "2013-05-23T05:54:50Z",
				unixTime: 1369288490264
			}
		],
		typeId: "<uuid>",
		deviceNetworkId: "<id>",
		virtual: false,
		primaryTileName: "contact",
		stateOverrides: [ ],
		permissions: "a"
		}
	]

### Device Details

Get details about a specific device (given its deviceId) along with its recent events.

	print api.device(device_id)

Example response:

	{
		device: {
			id: "<idHashString>",
			name: "SmartSense Multi",
			hubId: "<idHashString>",
			label: null,
			status: "ACTIVE",
			currentStates: [
				{
					name: "temperature",
					value: "64",
					unit: "F",
					date: "2013-05-23T06:04:50Z",
					unixTime: 1369289090908
				},
				{
					name: "threeAxis",
					value: "46,12,1016",
					unit: null,
					date: "2013-05-23T05:49:47Z",
					unixTime: 1369288187171
				},
				{
					name: "acceleration",
					value: "inactive",
					unit: null,
					date: "2013-05-23T06:04:50Z",
					unixTime: 1369289090908
				},
				{
					name: "contact",
					value: "closed",
					unit: null,
					date: "2013-05-23T06:04:50Z",
					unixTime: 1369289090908
				}
			],
			typeId: "<uuid>",
			deviceNetworkId: "<id>",
			virtual: false,
			primaryTileName: "contact",
			stateOverrides: [ ],
			permissions: "a"
		},
		events: [
		{
			id: "<uuid>",
			hubId: "<idHashString>",
			description: "SmartSense Multi was 64°F",
			displayed: true,
			linkText: "SmartSense Multi",
			date: "2013-05-23T05:59:50.721Z",
			unixTime: 1369288790721,
			value: "64",
			deviceId: "<idHashString>",
			unit: "F",
			name: "temperature",
			locationId: "<idHashString>"
		},
		{
			id: "<uuid>",
			hubId: "<idHashString>",
			description: "SmartSense Multi was active",
			displayed: true,
			linkText: "SmartSense Multi",
			date: "2013-05-23T05:49:17.822Z",
			unixTime: 1369288157822,
			value: "active",
			deviceId: "<idHashString>",
			name: "acceleration",
			locationId: "<idHashString>"
		},
		...
		]
	}

###Device events
Get the recent events for a given device (given its deviceId).

    #device_events =  api.device_events(device_id,max=1,source="DEVICE")
    #for e in device_events:
    #  print e

**Params**
`all=<boolean>`
`max=<number>`
`source=<boolean>`

###DEvice Roles

    #print api.device_roles(device_id)

###Device types
Get information about the types of installed devices.

    print api.device_types()


##Help

Please help!