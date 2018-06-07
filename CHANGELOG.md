# CHANGELOG

## Version 2.6.2

- Preventing long directions from exiting venue
- Making data available in beacons

## Version 2.6.1

- Use venue defaultCenter and defaultZoom
- Add Mapwize attribution link
- Fix latitude, longitude and floor can be negative in url parser
- Update doc to better fit in docs.mapwize.io

## Version 2.6.0

- Using outdoor.mapwize.io by default for the outdoor map
- The `outdoorMapProvider` map option has been removed. Use `OUTDOOR_TILES_URL` in the config instead.

## Version 2.5.0

- Changing floors behaviour to only have in the floor selector the floors for which there is a layer visible on the screen. Changed the behaviour of getFloors and floorsChange accordingly.

## Version 2.4.1

- Prevent crash on browsers not supporting playing with document.styleSheets
- Prevent firing venueExit with empty content
- Use api.mapwize.io by default
- Load first accessible universe by default
- Add demo with 2 maps on the same page

## Version 2.4.0

- Added support for decimal floors
- Url parser now returns the venue for place or beacon urls for which the place or beacon is not found
- Fix media permission issue on Firefox

## Version 2.3.7

- Fix bug with useBrowserLocation map option
- Add support for dashes (-) in object alias

## Version 2.3.6

- Improved floor control

## Version 2.3.5

- Fix analytics custom dimensions
- Fix order in places display
- Fix bug on order for outdoor layers
- Improved performances
- Improved API error handling

## Version 2.3.4

- Improved analytics
- Fix issue with followUserMode being disabled when it should not
- Fix compatibility issue with IE11

## Version 2.3.3

- Fix: main color also affect user position

## Version 2.3.2

- Fix bad style on direction change floor marker on some devices

## Version 2.3.1

- Added bounds map options

## Version 2.3

- Added support for external places
- Added support for changing user position and directions color
- Added support for customizing default marker icon

## Version 2.2

- Added support for outdoor layers to be displayed on all floors
- Added support for GeoJSON layers
- Added API method getVenuesForOrganization and search
- Improved documentation

## Version 2.1

- Migrating to Leaflet 1.0.3 from 0.7. This can bring breaking changes if you are using Leaflet features directly. Please refer to the [Leaflet documentation](http://leafletjs.com/reference-1.0.3.html). 
- Adding multi-point directions. It is now possible to optimize a path with multiple waypoints.
- Adding minZoom param on fitBounds
- Fix bad center on venue zoom on large venues
- Mapwize images (like qrCode and user position button) are now embedded in css
- New direction path style
- [Breaking change] Api.getDirections is now taking an array of waypoints (from, to, waypoints, directionOptions, cb). Backward compatibility has been kept on the map.showDirections function.
- General improvements and bugs fixes

## Version 2.0.2

- Optimized performances deep inside
- Added advanced parser for Mapwize URL
- Added configuration for outdoorMapProvider

## Version 1.7.1

- Renamed Mapwize.api to Mapwize.Api
- Introducing Mapwize.Location service
- Added support for isVisible and isClickable places
- Added support for custom connector icons
- Added callbacks in asynchronous methods

## Version 1.6.1

- Improvements to the initialization sequence
- Demo building now requires an accessKey

## Version 1.6

- Support for multilingual venues
- Support for storing custom "data" on venues, places, placeLists and beacons
- Added callback on Mapwize.map
- Introducing venueEnter and venueExit events

## Version 1.5

- Possibility to request shortest path to list of places (closest place of list is automatically selected)
- Places and layers are displayed at zoom 16 instead of 18
- Improved display of the outdoor places and directions
- Added Mapwize.api methods
- General improvements

## Version 1.4.4

- Bug fix getLayers
- Bug fix setStyle
- Bug fix scroll floor control

## Version 1.4.3

- Bug fix
- Improved caching
- Setting the right user position while loading direction URLs

## Version 1.4.2

- Places are displayed in the order defined on the server.

## Version 1.4.1

- Added possibility to display user heading
- Added a cache system to reduce the network traffic. Use map.refresh to force the refresh of the cache.
- Margins can be set to free the top and bottom of the map.
- Place style can be modifyed in real-time
