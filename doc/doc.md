# Mapwize.js

The SDK is built as a [Leaflet](http://leafletjs.com/) plugin. Leaflet is included in the package so you don't have to worry about importing it. Mapwize.js is all you need.

The package is exposed both in `L.Mapwize` and simply as `Mapwize`.

## Usage

Here are the specific instructions for using Mapwize. Please refer to the [Leaflet Doc](http://leafletjs.com/reference.html) for all the Leaflet related options.

### Display the Mapwize map
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

### Limit the visible area
The area the user can browse can be limited to a given bound. To do so, use the Leaflet option `maxBounds` or the method `setMaxBounds`. Example:

	var map = Mapwize.map('map', {
		maxBounds: [ [40.712, -74.227], [40.774, -74.125] ]
	});

### Center on position
You can center the map on a given position given by

- latitude
- longitude
- floor: if not set or if does not exist at that location, the floor is set to 'null'
- zoom: between 0 and 21

The floor related methods are specific to Mapwize. The other methods are inherited from Leaflet.

You can give the options at creation time:

	var map = Mapwize.map('map', {
		center: [40.712, -74.227],
		zoom: 19,
		floor: 2
	});

or dynamically:

	map.setView([40.712, -74.227], 19);
	map.panTo([40.712, -74.227]);
	map.setZoom(19);
	map.setFloor(2);
	
### Fit given area
You can set the map so that a given bount is completely displayed (inherited from Leaflet).

	map.fitBounds([
	    [40.712, -74.227],
    	[40.774, -74.125]
	]);

### Center map on place
You can center the map on a given Mapwize place. The floor is automatically selected based on the place.

	map.centerOnPlace('placeId');
		
### Listen for place click
An event is triggered every time a place is clicked.

	map.on('placeClick', function(e){
		console.log(e.place);
	});

### Listen for floors changed
An event is triggered every time the list of available at the currently viewed location is changed.

	map.on('floorsChanged', function(e){
		console.log('Available floors at position changed to ' + e.floors);
	});

### Listen for floor changed
An event is triggered every time the currently viewed floor is changed.

	map.on('floorChanged', function(e){
		console.log('Floor changed to ' + e.floor);
	});

### User position
By default, if the user position is available, it is displayed. You can prevent the user location from being displayed by setting the `showUserPosition` map option to false:

	var map = Mapwize.map('map', {
		showUserPosition: false
	});

If `showUserPosition` is not false, the control proposing the user to go to its position is displayed by default. You can disable it by setting the `showUserPositionControl` map option to false:

	var map = Mapwize.map('map', {
		showUserPositionControl: false
	});

By default, the user position is acquired from the web browser if available. You can disable it by setting the `useBrowserLocation` map option to false:

	var map = Mapwize.map('map', {
		useBrowserLocation: false
	});

You can manually set the user position by using the `setUserPosition` method:

	map.setUserPosition({
		latitude: 40.712, 
		longitude: -74.227,
		floor: 2,
		accuracy: 5
	});
		
The accuracy is specified in meters. At this point the accuracy is only on position. There is no accuracy on floor.
	
If the followUserMode is enabled, then this methods will have as consequence to move the map. Otherwize, only the user position dot will be moved.

You can manage the followUserMode using the following commands:

	map.getFollowUserMode()
	map.setFollowUserMode(true);
	
To center the map on the current user, you can use the `centerOnUser` method. The method takes a minZoom parameter. If the current zoom of the map is lower than minZoom, then the map is zoomed to minZoom.	

	map.centerOnUser(19);


### QR-code
If you have retrieved a QR-code and want to pass it to the SDK, you can use the following method and pass the QR-code payload:

	map.qrcode('payload')
	
To add the QR-code scan button to the map (by default at bottom left):

	var qrcodeControl = Mapwize.qrcodeControl({
		onClick: function() {
			/* scan the QR code and retrieve payload */  
			map.qrcode(payload);
		}
	});
	qrcodeControl.addTo(map);


### Access Key
If you want to access private buildings, you need to specify the related access key.

	map.access('key', function (result) {
		if (result) {
			// NO error, key is valid
		}
		else {
			// ERROR, key is not valid
		}
	});
	
To add the key button to the map (by default at top right):

	var accessControl = Mapwize.accessControl({
		onClick: function() {
			/* enter the access key */  
			map.access(key, function (result) {
				if (result) {
					// NO error, key is valid
                }
                else {
                	// ERROR, key is not valid
                }
			});
		}
	});
	accessControl.addTo(map);

