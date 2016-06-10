# CHANGELOG

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