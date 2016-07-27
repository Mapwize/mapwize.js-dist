# Mapwize.js

The SDK is built as a [Leaflet](http://leafletjs.com/) plugin. Leaflet is included in the package so you don't have to worry about importing it. Mapwize.js is all you need.

The package is exposed both in `L.Mapwize` and simply as `Mapwize`.

Here are the specific instructions for using Mapwize. Please refer to the [Leaflet Doc](http://leafletjs.com/reference.html) for all the Leaflet related options.

----------

## Summary
* <a href="#_display-map">Display the Mapwize map</a>
* <a href="#_map-constructor">Map constructor</a>
* <a href="#_limit-area">Limit the visible area</a>
* <a href="#_center-map">Center the map</a>
	* <a href="#_center-coordinates">Center on coordinates</a>
	* <a href="#_center-venue">Center on venue</a>
	* <a href="#_center-place">Center on place</a>
	* <a href="#_fit-area">Fit given area</a>
* <a href="#_display-directions">Display directions</a>
* <a href="#_user-position">User position</a>
	* <a href="#_display-position">Display user position</a>
	* <a href="#_use-browser">Use browser location</a>
	* <a href="#_follow-user">Follow user mode</a>
	* <a href="#_center-user">Center map on user position</a>
	* <a href="#_get-user">Get user position</a>
	* <a href="#_set-user">Setting the user position</a>
	* <a href="#_multiple-sources">Using multiple measurement sources</a>
	* <a href="#_set-heading">Setting the user heading</a>
* <a href="#_urls">Setting the map based on URL</a>
* <a href="#_markers">Adding markers</a>
* <a href="#_events">Listen for events</a>
	* <a href="#_event-click">`click`</a>
	* <a href="#_event-contextmenu">`contextmenu`</a>
	* <a href="#_event-moveend">`moveend`</a>
	* <a href="#_event-placeClick">`placeClick`</a>
	* <a href="#_event-floorsChange">`floorsChange`</a>
	* <a href="#_event-floorChange">`floorChange`</a>
	* <a href="#_event-userPositionChange">`userPositionChange`</a>
	* <a href="#_event-directionsStart">`directionsStart`</a>
	* <a href="#_event-directionsStop">`directionsStop`</a>
	* <a href="#_event-followUserModeChange">`followUserModeChange`</a>
	* <a href="#_event-venueEnter">`venueEnter`</a>
	* <a href="#_event-venueExit">`venueExit`</a>
	* <a href="#_event-marginsChange">`marginsChange`</a>
	* <a href="#_event-preferredLanguageChange">`preferredLanguageChange`</a>
* <a href="#_qrCode">QR-code</a>
* <a href="#_access-key">Access Key</a>
* <a href="#_cache">Cache</a>
* <a href="#_margins">Margins</a>
* <a href="#_place-style">Modify Place Style</a>
* <a href="#_multilingual">Multilingual venues</a>
* <a href="#_custom-data">Adding custom data to objects</a>
* <a href="#_api">Api</a>
	* <a href="#_api-venues">Venues</a>
	* <a href="#_api-places">Places</a>
	* <a href="#_api-layers">Layers</a>
	* <a href="#_api-connectors">Connector places</a>
	* <a href="#_api-beacons">Beacons</a>
	* <a href="#_api-placeTypes">Place types</a>
	* <a href="#_api-placeLists">Place lists</a>

----------

## <a id="_display-map"></a>Display the Mapwize map
The simplest way to display Mapwize:

	<html>
		<head>
			<script src="mapwize.js" type="text/javascript"></script>
			<link rel="stylesheet" href="mapwize.css" />
			<style>
                #map {
                    height: 500px;
                }
            </style>
		</head>
		<body>

			<div id="map"></div>

			<script>
				var map = Mapwize.map('map', {
        			apiKey: {{YOUR MAPWIZE API KEY HERE}}
    			}, function () {
    			    console.log('map is now loaded');
    			});
			</script>

		</body>
	</html>

There has to be a div for where the map is going to go. This div can have a css style like for example 
	
	#map {
    	position:absolute; top:0; bottom:0; width:100%; height: 100%;
    }

for a full screen display. Pay attention to have `<div id="map"></div>` and not `<div id="map"/>` as the second case will NOT work. This seems to be linked to Leaflet.

On the full map, everything is displayed:

- Layers with floor selector
- Places with shapes and markers
- Button to position yourself and enable the tracking

----------

## <a id="_map-constructor"></a>Map constructor

    Mapwize.map(mapId, options, callback);
    
    mapId: The id of the html div where the map should be created
    options: The options for the map (see further for all possible options)
    callback: function(err) The callback function called when the map is initialized, with errors if any.

All of possible options (in addition to <a href="http://leafletjs.com/reference.html#map-options">all leaflet options</a>):

 - `apiKey` *(String, required)* Your Mapwize api key, find it in the Admin portal under the  **Applications** menu
 - <a href="#_access-key">`accessKey`</a> *(String, optionnal)* A Mapwize access key to access private venues. If necessary, it can be generated in the Admin portal.
 - `displayOutdoorMap` *(Boolean, optionnal, default: true)* If `true`, the default background map is displayed. Set to `false` if you want to use another background map.
 - `displayLayers` *(Boolean, optionnal, default: true).* If `true`, Mapwize **layers** are displayed on the map
 - `displayVenues` *(Boolean, optionnal, default: true).* If `true`, Mapwize **venues** are displayed on the map
 - `displayPlaces` *(Boolean, optionnal, default: true).* If `true`, Mapwize **places** are displayed on the map
 - `displayPlacesOptions` *(Object, optionnal, default: {})*
 - `displayConnectors` *(Boolean, optionnal, default: true)* If `true`, Mapwize **connectors** are displayed on the map
 - `displayFloorControl` *(Boolean, optionnal, default: true)* If `true`, the Floor Control is added on the map
 - `floorControlOptions` *(Object, optionnal, default: {})* 
 - `cacheParams` *(Object, optionnal, default: {})* Sent with each Mapwize api request. <a href="https://github.com/Mapwize/mapwize-api-doc">See Mapwize api doc</a>
 - `floor` *(Integer, optionnal, default: 0)* <a href="#_center-coordinates">Sets the initial floor</a>
 - `showUserPosition` *(Boolean, optionnal, default: true)* <a href="#_display-position">See the User Position section</a>
 - `showUserPositionControl` *(Boolean, optionnal, default: true)* <a href="#_display-position">See the User Position section</a>
 - `useBrowserLocation` *(Boolean, optionnal, default: true)* <a href="#_use-browser">See the User Position section</a>
 - `marginTop` *(Integer, optionnal, default: 0)* <a href="#_margins">See the Margins section</a>
 - `marginBottom` *(Integer, optionnal, default: 0)* <a href="#_margins">See the Margins section</a>
 - `language` *(String, optionnal, default: null)* <a href="#_multilingual">See the Multilingual section</a>

## <a id="_limit-area"></a>Limit the visible area
The area the user can browse can be limited to a given bound. To do so, use the Leaflet map options `maxBounds` and `minZoom` at initialization of the map. Example:

	var map = Mapwize.map('map', {
		maxBounds: [ [40.712, -74.227], [40.774, -74.125] ],
		minZoom: 18
	});

----------

## <a id="_center-map"></a>Center the map

### <a id="_center-coordinates"></a>Center on coordinates
You can center the map on given coordinates defined by

- latitude
- longitude
- floor: if not set or if does not exist at that location, the floor is set to 'null'
- zoom: between 0 and 21, default 19

You can give the options at creation time:

	var map = Mapwize.map('map', {
		center: [40.712, -74.227],
		zoom: 19,
		floor: 2
	});

Or dynamically:

	map.centerOnCoordinates(latitude, longitude, [floor], [zoom]);
	map.setFloor(2);
	map.setZoom(19);

### <a id="_center-venue"></a>Center on venue
You can center the map on a venue by calling `centerOnVenue` with either a venueId or a venue object. Venue objects can be retrieved using the `api.getVenue` method.

	map.centerOnVenue('venueId');
	map.centerOnVenue(venue);
	
### <a id="_center-place"></a>Center on place
You can center the map on a given place by calling `centerOnPlace` with either a placeId or a place object. The floor is automatically selected based on the place. Place objects can be retrieved using the `api.getPlace` method.

	map.centerOnPlace('placeId');
	map.centerOnPlace(place);
	
### <a id="_fit-area"></a>Fit a given area
You can set the map so that a given bount is completely displayed.

	map.fitBounds([
	    [40.712, -74.227],
    	[40.774, -74.125]
	]);

----------

## <a id="_display-directions"></a>Display directions

You can display directions on the map between 2 points.

	map.showDirections(from, to, options, callback)
	
The from and the to parameters are objects which can have the following properties:

	{
		placeId: string (the Id of a place. If used, latitude/longitude/floor are ignored)
		placeListID: string (the Id of a placeList. If used, placeId/latitude/longitude/floor are ignored)
		latitude: number (if used, all latitude/longitude/floor are required)
		longitude: number (if used, all latitude/longitude/floor are required)
		floor: number (if used, all latitude/longitude/floor are required)
	}

The options parameter is an object {} however no particular properties are supported at the moment.

The callback is a function returning an error object or directions object

	callback = function(err, directions)
	
Err is null if there was no error and a direction is displayed.

Directions is an object containing the folloing properties:

	{
		from: from object as above,
		to: to object as above,
		distance: total distance in meters,
		bounds: bounds containing the total route,
		routes: [ a list of routes on different floors
			floor: the floor number of the route
			path: the list of coordinates on the way points
			distance: the distance of the route in meter
			bounds: the bounds of the route
			isStart: if the route of the first one
			isEnd: if the route is the last one
			fromFloor: the floor of the route before
			toFloor: the floor of the route after
		]
	}

To remove the directions, use the method `stopDirections`

	map.stopDirections();

----------

## <a id="_user-position"></a>User position

### <a id="_display-position"></a>Display user position

By default, if the user position is available, it is displayed. You can prevent the user location from being displayed by setting the `showUserPosition` map option to false:

	var map = Mapwize.map('map', {
		showUserPosition: false
	});

If `showUserPosition` is not false, the control proposing the user to go to its position is displayed by default. You can disable it by setting the `showUserPositionControl` map option to false:

	var map = Mapwize.map('map', {
		showUserPositionControl: false
	});

### <a id="_use-browser"></a>Use browser location

By default, the user position is acquired from the web browser if available. You can disable it by setting the `useBrowserLocation` map option to false:

	var map = Mapwize.map('map', {
		useBrowserLocation: false
	});

### <a id="_follow-user"></a>Follow user mode

The follow user mode allows to have the map moving as the position is updated.

You can manage the followUserMode using the following commands:

	map.getFollowUserMode()
	map.setFollowUserMode(true/false);
	
### <a id="_center-user"></a>Center map on user position

To center the map on the current user positon, you can use the `centerOnUser` method. The method takes a minZoom parameter. If the current zoom of the map is lower than minZoom, then the map is zoomed to minZoom.	

	map.centerOnUser(19);

### <a id="_get-user"></a>Get user position

You can get the current user positon using the method

	map.getUserPosition()
	
The returned object has the following properties:

	{
		latitude: the latitude
		longitude: the longitude
		floor: the floor (null if ouside a building or unknown)
		accuracy: the radius in meter in which the user has a high probability to actually be, on the current floor. There is no vertical accuracy.
		validUntil:  timestamp until when the position is valid. Used for example for sources like QR-code which do not necesseraly get continuous measurements.
		source: a string describing the source of the measurement.
	}
 
### <a id="_set-user"></a>Setting the user position

You can manually set the user position by using the `setUserPosition` method:

	map.setUserPosition({
		latitude: 40.712, 
		longitude: -74.227,
		floor: 2,
		accuracy: 5
	});
		
The accuracy is specified in meters. At this point the accuracy is only on horizontal position. There is no accuracy on floor.
	
If the followUserMode is enabled, then this methods will have as consequence to move the map. Otherwize, only the user position dot will be moved.

When the setUserPosition method is used, the position of the user is locked on the provided position until setUserPosition is called again. If you then want to unlock the positon and use the default position given by the browser for example, then you need to call

	map.unlockUserPosition();
	
### <a id="_multiple-sources"></a>Using multiple measurement sources

In many situations, many positioning methods can be combined in order to define the most probable position of the user, for example GPS, iBeacons, QR-Code, Wifi, Lifi, ...

To handle this situation, Mapwize allows you to send different measurements from different sources to the SDk.

To do so, use the method

	map.newUserPositionMeasurement(measurement)

where measurement is an object with the following properties

	{
		latitude
		longitude
		floor
		accuracy
		validity: the time in milliseconds the measurement is valid. This allows the use of single measurement methods like the scan of a code, for which no update is provided as the user moves. Set validity to null for infinite.
		source: a string describing the source of the measurement. Each time a new measurement arrives, it discards automatically the previous measurement from the same source.
	}

You can lock the user position to the current position using the method lockUserPosition. When the position is locked, all measurements are ignored until the method unlockUserPosition is called.

The method setUserPosition overrules all the previous rules and sets the user position until unlockUserPosition is called.

### <a id="_set-heading"></a>Setting the user heading
When a compass is available, it can be interesting to display the direction the user is looking. To do so, the method setUserHeading can be used, giving it an angle in degree. Example if the user is looking south:

	map.setUserHeading(180);
	
To remove the display of the compass, simply set the angle to null.

----------

## <a id="_urls"></a>Setting the map based on URL

The Mapwize URL allows to refer positions, venues, places and directions so they can be displayed uniformly on every device, web and mobile.

The same URLs can be typed in a web browser or scanned as QR-code, using the Mapwize apps or the SDK.

The URLs always start with http://mwz.io/

To load a URL, use the following method:

	map.loadURL('url')

The complete documentation regarding the URL format can be found [in the mapwize-url-scheme repository on github](https://github.com/Mapwize/mapwize-url-scheme).

----------

## <a id="_markers"></a>Adding markers

You can add markers on the map to show a position of interest. At this points, marker are static elements and users cannot interact with them.

To add a marker, use the function

	map.addMarker(position)

where position is an object with the following properties:

	{
		placeId: string (the Id of a place. If used, latitude/longitude/floor are ignored)
		latitude: number (if used, longitude is required)
		longitude: number (if used, latitude is required)
		floor: number (can be null) 
	}
	
To remove all the markers at once, use the function

	map.removeMarkers()

At this point, it is not possible to only remove a single marker.

----------

## <a id="_events"></a>Listen for events

The map will emit various events you can listen to.

### <a id="_event-click"></a>click
Fired when the user clicks (or taps) the map.

	map.on('click', function(e){
		console.log(e);
	});

### <a id="_event-contextmenu"></a>contextmenu
Fired when the user pushes the right mouse button on the map. Also fired on mobile when the user holds a single touch for a second (also called long press).

	map.on('contextmenu', function(e){
		console.log(e);
	});

### <a id="_event-moveend"></a>moveend
Fired when the view of the map stops changing (e.g. user stopped dragging the map).

	map.on('moveend', function(e){
		console.log(e);
	});
		
### <a id="_event-placeClick"></a>place click
Fired when a place is clicked.

	map.on('placeClick', function(e){
		console.log(e.place);
	});

### <a id="_event-floorsChange"></a>floors change
Fired when the list of available floors at the currently viewed location is changed.

	map.on('floorsChange', function(e){
		console.log('Available floors at currently viewed location changed to ' + e.floors);
	});

### <a id="_event-floorChange"></a>floor change
Fired when the currently viewed floor is changed.

	map.on('floorChange', function(e){
		console.log('Floor changed to ' + e.floor);
	});

### <a id="_event-userPositionChange"></a>user position change
Fired when the user position has changed.

	map.on('userPositionChange', function(e){
		console.log('User position changed to ' + e.userPosition);
	});

### <a id="_event-directionsStart"></a>directions start
Fired when directions are displayed on the map.

	map.on('directionsStart', function(e){
		console.log('Directions have been loaded');
	});

### <a id="_event-directionsStop"></a>directions stop
Fired when directions are stopped and not displayed on the map anymore.

	map.on('directionsStop', function(e){
		console.log('Directions have stopped');
	});

### <a id="_event-followUserModeChange"></a>followUserMode change
Fired when FollowUserMode changed.

	map.on('followUserModeChange', function(e){
		console.log('followUserMode new value: ' + e.followUserMode);
	});

### <a id="_event-venueEnter"></a>venue enter
Fired when a venue is displayed (zoom level >= 16 and center of the map inside the venue)

	map.on('venueEnter', function(e){
		console.log('Venue entered: ' + e.venue);
	});

### <a id="_event-venueExit"></a>venue exit
Fired when leaving the venue that was previously entered 

	map.on('venueExit', function(e){
		console.log('Venue exited: ' + e.venue);
	});
	
### <a id="_event-marginsChange"></a>margins change
Fired when the map margins have changed

	map.on('marginsChange', function(e){
		console.log('margins: ' + e.margins);
	});
	
### <a id="_event-preferredLanguageChange"></a>preferred language change
Fired when the map preferred language has changed

	map.on('preferredLanguageChange', function(e){
		console.log('Language: ' + e.language);
	});

----------

## <a id="_qrCode"></a>QR-code
If you have retrieved a QR-code and want to pass it to the SDK, you can use the following method and pass the QR-code payload:

	map.loadURL('payload')
	
To add the QR-code scan button to the map (by default at bottom left):

	var qrcodeControl = Mapwize.qrcodeControl({
		onClick: function() {
			/* scan the QR code and retrieve payload */  
			map.loadURL(payload);
		}
	});
	qrcodeControl.addTo(map);

----------

## <a id="_access-key"></a>Access Key
If you want to access private buildings, you need to specify the related access key.

You can pass the access key directly when initializing the map:

	var map = Mapwize.map('map', {
		accessKey: 'YOUR KEY'
	});

Or you can use the access method

	map.access('key', function (result) {
		if (result) {
			// NO error, key is valid
		}
		else {
			// ERROR, key is not valid
		}
	});

----------

## <a id="_cache"></a>Cache
To prevent too many network requests while browsing the map, the SDK keeps a cache of some data it already downloaded.

The Time To Live of the cache is 5 minutes.

If you want to force the map to refresh the cache and update itself, you can call the refresh method anytime.

	map.refresh();

----------

## <a id="_margins"></a>Margins
It often happens that part of the map is hidden by banners or controls on the top or on the bottom. For example, if you display a banner to show the details of the place you just clicked on, it's better to display the banner on top of the map than having to resize the map.

However, you want to make sure that the Mapwize controls are always visible, like the followUserMode button and the floor selector. Also, that if you make a fitBounds, the area will be completely in the visible part of the map.

For this purpose, you can set a top and a bottom margin on the map. We garantee that nothing important will be displayed in those margin areas.

To set the margins, you can pass them in pixels when you intialize the map:

	var map = Mapwize.map('map', {
		marginTop: 50,
		marginBottom: 50
	});

Or you can change them at runtime

	map.setTopMargin(50);
	map.setBottomMargin(50);

----------

## <a id="_place-style"></a>Modify Place Style
The style of a place can be modifyed directly within the SDK and can then override the style sent by the server. This is the best way to make changes in real-time on the map as it does not require to contact the Mapwize servers. For example, this can be used to display the availability of a meeting room.

	map.setPlaceStyle(placeId, style);
	
where style is an object with the format:

	{
		markerUrl: string (An url to the icon of the marker. Must be an image, ideally png, square, 100*100 pixels),
        strokeColor: string (The color of the shape border as #hex),
        strokeOpacity: number (The opacity of the border, between 0 and 1),
        strokeWidth: number (The width of the border),
        fillColor: string (The color of the inside of the shape as #hex),
        fillOpacity: number (The opacity of the inside, between 0 and 1),
        labelBackgroundColor: string (The color of the backgroud of the label as #hex),
        labelBackgroundOpacity: number (The opacity of the background of the label, between 0 and 1)
	}

example:

	{
		markerUrl: 'http://myserver.com/image.png',
       	strokeColor: '#C51586',
        strokeOpacity: 1,
        strokeWidth: 2,
        fillColor: '#FFFFFF',
        fillOpacity: 0.3,
        labelBackgroundColor: null,
        labelBackgroundOpacity: null
	}

Note that if a parameter is null, the value defined on the server will be used.

----------

## <a id="_multilingual"></a>Multilingual venues
Venues can support multiple languages.
By default, venues are displayed in their default language configured on the backend-side.
Using the function

    map.setPreferredLanguage(language);
    
it is possible to set the preferred language of the user. If a venue supports the preferred language, it will be displayed in that language.
Otherwise, it will be displayed in the default language.

Languages are defined using 2 letter codes like 'en', 'fr', ...

Setting the preferred language to null displays all venues in their default language.

----------

## <a id="_custom-data"></a>Adding custom data to objects
To define specific behavior in your app for venues, places, placeLists or beacons, it is handy to attach custom data to those objects.
Data can be added using the API or the backend interface.
Data are retrieved in the objects under the property "data".

----------

## <a id="_api"></a>Api

You can access to the Mapwize api with the SDK by using `Mapwize.Api`.

### <a id="_api-venue"></a>Venues

#### get

    Mapwize.Api.venues.get(options, callback);

##### Arguments

 1. `options`: *object|String*
 2. `callback`: *function(err, venue)* `venue` is an **venue** object.

`options` can be a string as venueId or an object as:

    {
	    [id|venueId]: (String) an id of venue
	    [alias|venueAlias]: (String) an alias of venue
	    [name|venueName]: (String) a name of venue
    }

Priority rules:
- if you provide id, alias and name are ignored
- if you provide alias, name is ignored

##### Example

    // Using venue id
    Mapwize.Api.venues.get('aValidVenueId', function (err, venue) {
	    console.log(venue);
    });
    
    // Using venue alias
    Mapwize.Api.venues.get({alias: 'aValidVenueAlias'}, function (err, venue) {
	    console.log(venue);
    });

#### list

Get a list of venues 

    Mapwize.Api.venues.list(options, callback);

##### Arguments

 1. `options`: *object*
 2. `callback`: *function(err, venues)* `venues` is an array of **venue** object.

`options` is an object as:

    {
	    // you should use both together
	    latitudeMin: (number)
	    latitudeMax: (number)
	    
	    // you should use both together
	    longitudeMin: (number)
	    longitudeMax: (number)
    }

##### Example

    // with empty options
    Mapwize.Api.venues.list({}, function (err, venues) {
	    // venues contain all venues you can access
	    console.log(venues);
    });
    
    // In defined area
    Mapwize.Api.venues.list({latitudeMin: 0, latitudeMax: 1, longitudeMin: 0, longitudeMax: 1}, function (err, venues) {
	    // venues contain all venues in the defined area
	    console.log(venues);
    });

### <a id="_api-places"></a>Places

#### get

    Mapwize.Api.places.get(options, callback);

##### Arguments

 1. `options`: *object|String*
 2. `callback`: *function(err, place)* `place` is an **place** object.

`options` can be a string as placeId or an object as:

    {
	    [id|placeId]: (String) an id of place
	    [alias|placeAlias]: (String) an alias of place
	    [name|placeName]: (String) a name of place
	    
	    venueId: (String) required if you use name or alias
    }

Priority rules:
- if you provide id, alias and name are ignored
- if you provide alias, name is ignored

##### Example

    // Using venue id
    Mapwize.Api.places.get('aValidPlaceId', function (err, place) {
	    console.log(place);
    });
    
    // Using venue alias
    Mapwize.Api.places.get({alias: 'aValidPlaceAlias', venueId: 'aValideVenueId'}, function (err, place) {
	    console.log(place);
    });

#### list

Get a list of places 

    Mapwize.Api.places.list(options, callback);

##### Arguments

 1. `options`: *object*
 2. `callback`: *function(err, places)* `places` is an array of **place** object.

`options` is an object as:

    {
	    // get only places in placeList
	    // if used, other params are ignored
	    placeListId: (String)
	    
	    // you should use both together
	    latitudeMin: (number)
	    latitudeMax: (number)
	    
	    // you should use both together
	    longitudeMin: (number)
	    longitudeMax: (number)
	    
	    floor: (Integer)
    }

##### Example

    // with empty options
    Mapwize.Api.places.list({}, function (err, places) {
	    // places contain all places you can access
	    console.log(places);
    });
    
    // In defined area
    Mapwize.Api.places.list({latitudeMin: 0, latitudeMax: 1, longitudeMin: 0, longitudeMax: 1, floor: 1}, function (err, places) {
	    // places contain all places in the defined area on floor 1 (and floor outdoor (null))
	    console.log(places);
    });

### <a id="_api-layers"></a>Layers

#### list

Get a list of layers 

    Mapwize.Api.layers.list(options, callback);

##### Arguments

 1. `options`: *object*
 2. `callback`: *function(err, layers)* `layers` is an array of **layer** object.

`options` is an object as:

    {
	    // you should use both together
	    latitudeMin: (number)
	    latitudeMax: (number)
	    
	    // you should use both together
	    longitudeMin: (number)
	    longitudeMax: (number)
	    
	    floor: (Integer)
    }

##### Example

    // with empty options
    Mapwize.Api.layers.list({}, function (err, layers) {
	    // layers contain all layers you can access
	    console.log(layers);
    });
    
    // In defined area
    Mapwize.Api.layers.list({latitudeMin: 0, latitudeMax: 1, longitudeMin: 0, longitudeMax: 1, floor: 1}, function (err, layers) {
	    // layers contain all layers in the defined area on floor 1 (and floor outdoor (null))
	    console.log(layers);
    });

### <a id="_api-connectors"></a>Connector places

#### list

Get a list of venues 

    Mapwize.Api.connectorPlaces.list(options, callback);

##### Arguments

 1. `options`: *object*
 2. `callback`: *function(err, connectorPlaces)* `connectorPlaces` is an array of **connectorPlace** object.

`options` is an object as:

    {
	    // you should use both together
	    latitudeMin: (number)
	    latitudeMax: (number)
	    
	    // you should use both together
	    longitudeMin: (number)
	    longitudeMax: (number)
	    
	    floor: (Integer)
    }

##### Example

    // with empty options
    Mapwize.Api.connectorPlaces.list({}, function (err, connectorPlaces) {
	    // connectorPlaces contain all connectorPlaces you can access
	    console.log(connectorPlaces);
    });
    
    // In defined area
    Mapwize.Api.connectorPlaces.list({latitudeMin: 0, latitudeMax: 1, longitudeMin: 0, longitudeMax: 1, floor: 1}, function (err, connectorPlaces) {
	    // connectorPlaces contain all connectorPlaces in the defined area on floor 1 (and floor outdoor (null))
	    console.log(connectorPlaces);
    });

### <a id="_api-beacons"></a>Beacons

#### get

    Mapwize.Api.beacons.get(options, callback);

##### Arguments

 1. `options`: *object|String*
 2. `callback`: *function(err, beacons)* `beacons` is an **beacon** object.

`options` can be a string as beaconId or an object as:

    {
	    [id|beaconId]: (String) an id of beacon
	    payload: (String) a payload of beacon
	    [alias|beaconAlias]: (String) a alias of beacon
	    [name|beaconName]: (String) a name of beacon
	    venueId: (String) required if you use name or alias
    }

Priority rules:
- if you provide id: payload, alias and name are ignored
- if you provide payload: alias and name are ignored
- if you provide alias: name is ignored

##### Example

    // Using venue id
    Mapwize.Api.beacons.get('aValidBeaconId', function (err, beacon) {
	    console.log(beacon);
    });
    
    // Using venue alias
    Mapwize.Api.beacons.get({alias: 'aValidBeaconAlias', venueId: 'aValidVenueId'}, function (err, beacon) {
	    console.log(beacon);
    });

#### list

Get a list of beacons 

    Mapwize.Api.beacons.list(options, callback);

##### Arguments

 1. `options`: *object*
 2. `callback`: *function(err, beacons)* `beacons` is an array of **beacon** object.

`options` is an object as:

    {
	    venueId: (String, required) a venue id
    }

##### Example

    // with empty options
    Mapwize.Api.beacons.list({venueId: 'aValidVenueId'}, function (err, beacons) {
	    // beacons contain all beacons you can access in the venue
	    console.log(beacons);
    });

### <a id="_api-placeTypes"></a>Place types

#### get

    Mapwize.Api.placeTypes.get(options, callback);

##### Arguments

 1. `options`: *String* a placeType id
 2. `callback`: *function(err, placeType)* `placeType` is an **placeType** object.

##### Example

    Mapwize.Api.placeTypes.get('aValidePlaceTypeId', function (err, placeType) {
	    console.log(placeType);
    });

### <a id="_api-placeLists"></a>Place Lists

#### get

    Mapwize.Api.placeLists.get(options, callback);

##### Arguments

 1. `options`: *object|String*
 2. `callback`: *function(err, placeList)* `placeList` is an **placeList** object.

`options` can be a string as placeListId or an object as:

    {
	    [id|listId]: (String) an id of venue
	    [alias|listAlias]: (String) an alias of venue
	    [name|listName]: (String) a name of venue
	    venueId: (String) required if you use name or alias
    }

Priority rules:
- if you provide id: alias and name are ignored
- if you provide alias: name is ignored

##### Example

    // Using placeList id
    Mapwize.Api.placeLists.get('aValidePlaceListId', function (err, placeList) {
	    console.log(placeList);
    });
    
    // Using placeList alias
    Mapwize.Api.placeLists.get({alias: 'aValidePlaceListAlias', venueId: 'aValidVenueId'}, function (err, placeList) {
	    console.log(placeList);
    });

#### list

Get a list of placeLists 

    Mapwize.Api.placeLists.list(options, callback);

##### Arguments

 1. `options`: *object*
 2. `callback`: *function(err, placeLists)* `placeLists` is an array of **placeList** object.

`options` is an object as:

    {
	    venueId: (String, required)
    }

##### Example

    // with empty options
    Mapwize.Api.placeLists.list({venueId: 'aValidVenueId'}, function (err, placeLists) {
	    // placeLists contain all placeLists you can access in the venue
	    console.log(placeLists);
    });