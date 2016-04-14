# Mapwize.js

The SDK is built as a [Leaflet](http://leafletjs.com/) plugin. Leaflet is included in the package so you don't have to worry about importing it. Mapwize.js is all you need.

The package is exposed both in `L.Mapwize` and simply as `Mapwize`.

Here are the specific instructions for using Mapwize. Please refer to the [Leaflet Doc](http://leafletjs.com/reference.html) for all the Leaflet related options.

## Display the Mapwize map
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
				var map = Mapwize.map('map' {
        			apiKey: {{YOUR API KEY HERE}}
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

## Limit the visible area
The area the user can browse can be limited to a given bound. To do so, use the Leaflet map options `maxBounds` and `minZoom` at initialization of the map. Example:

	var map = Mapwize.map('map', {
		maxBounds: [ [40.712, -74.227], [40.774, -74.125] ],
		minZoom: 18
	});

## Center the map

### Center on coordinates
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

### Center on venue
You can center the map on a venue by calling `centerOnVenue` with either a venueId or a venue object. Venue objects can be retrieved using the `api.getVenue` method.

	map.centerOnVenue('venueId');
	map.centerOnVenue(venue);
	
### Center on place
You can center the map on a given place by calling `centerOnPlace` with either a placeId or a place object. The floor is automatically selected based on the place. Place objects can be retrieved using the `api.getPlace` method.

	map.centerOnPlace('placeId');
	map.centerOnPlace(place);
	
### Fit given area
You can set the map so that a given bount is completely displayed.

	map.fitBounds([
	    [40.712, -74.227],
    	[40.774, -74.125]
	]);
	
## Display directions

You can display directions on the map between 2 points.

	map.showDirections(from, to, options, callback)
	
The from and the to parameters are objects which can have the following properties:

	{
		placeId: string (the Id of a place. If used, latitude/longitude/floor are ignored)
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
	

## User position

### Display user position

By default, if the user position is available, it is displayed. You can prevent the user location from being displayed by setting the `showUserPosition` map option to false:

	var map = Mapwize.map('map', {
		showUserPosition: false
	});

If `showUserPosition` is not false, the control proposing the user to go to its position is displayed by default. You can disable it by setting the `showUserPositionControl` map option to false:

	var map = Mapwize.map('map', {
		showUserPositionControl: false
	});

### Use browser location

By default, the user position is acquired from the web browser if available. You can disable it by setting the `useBrowserLocation` map option to false:

	var map = Mapwize.map('map', {
		useBrowserLocation: false
	});

### Follow user mode

The follow user mode allows to have the map moving as the position is updated.

You can manage the followUserMode using the following commands:

	map.getFollowUserMode()
	map.setFollowUserMode(true/false);
	
### Center map on user position

To center the map on the current user positon, you can use the `centerOnUser` method. The method takes a minZoom parameter. If the current zoom of the map is lower than minZoom, then the map is zoomed to minZoom.	

	map.centerOnUser(19);

### Get user position

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
 
### Setting the user position

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
	
### Using multipe measurement sources

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

### Setting the user heading
When a compass is available, it can be interesting to display the direction the user is looking. To do so, the method setUserHeading can be used, giving it an angle in degree. Example if the user is looking south:

	map.setUserHeading(180);
	
To remove the display of the compass, simply set the angle to null.
 
## Setting the map based on URL

The Mapwize URL allows to refer positions, venues, places and directions so they can be displayed uniformly on every device, web and mobile.

The same URLs can be typed in a web browser or scanned as QR-code, using the Mapwize apps or the SDK.

The URLs always start with http://mwz.io/

To load a URL, use the following method:

	map.loadURL('url')

The complete documentation regarding the URL format can be found [in the mapwize-url-scheme repository on github](https://github.com/Mapwize/mapwize-url-scheme).


## Adding markers

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


## Listen for events

The map will emit various events you can listen to.

### click
Fired when the user clicks (or taps) the map.

	map.on('click', function(e){
		console.log(e);
	});

### contextmenu
Fired when the user pushes the right mouse button on the map. Also fired on mobile when the user holds a single touch for a second (also called long press).

	map.on('contextmenu', function(e){
		console.log(e);
	});

### moveend
Fired when the view of the map stops changing (e.g. user stopped dragging the map).

	map.on('moveend', function(e){
		console.log(e);
	});
		
### place click
Fired when a place is clicked.

	map.on('placeClick', function(e){
		console.log(e.place);
	});

### floors change
Fired when the list of available floors at the currently viewed location is changed.

	map.on('floorsChange', function(e){
		console.log('Available floors at currently viewed location changed to ' + e.floors);
	});

### floor change
Fired when the currently viewed floor is changed.

	map.on('floorChange', function(e){
		console.log('Floor changed to ' + e.floor);
	});

### user position change
Fired when the user position has changed.

	map.on('userPositionChange', function(e){
		console.log('User position changed to ' + e.userPosition);
	});

### directions start
Fired when directions are displayed on the map.

	map.on('directionsStart', function(e){
		console.log('Directions have been loaded');
	});

### directions stop
Fired when directions are stopped and not displayed on the map anymore.

	map.on('directionsStop', function(e){
		console.log('Directions have stopped');
	});

### followUserMode change
Fired when FollowUserMode changed.

	map.on('followUserModeChange', function(e){
		console.log('followUserMode new value: ' + e.followUserMode);
	});


## QR-code
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


## Access Key
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

## Cache
To prevent too many network requests while browsing the map, the SDK keeps a cache of some data it already downloaded.

The Time To Live of the cache is 5 minutes.

If you want to force the map to refresh the cache and update itself, you can call the refresh method anytime.

	map.refresh();
	
## Margins
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
	
## Modify Place Style
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
