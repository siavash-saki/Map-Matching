# Map-Matching

Examples demonstrating how to use Valhalla Meili Map-Matching service

# Building a Valhalla Routing Server

There are two ways to build a Valhalla routing engine. 
The first one is building from the source. 
It is explained on the [Valhalla GitHub page](https://github.com/valhalla/valhalla). 
The second way is to use the docker container. 
Valhalla's original docker containers can be found [here](https://hub.docker.com/r/valhalla/valhalla/tags). 
But it is recommended to use the [GIO OPS docker container](https://github.com/gis-ops/docker-valhalla). 
It is more straightforward. 
In order to build a Valhalla engine using docker, you need the latest `docker` and `docker-compose` running on an `Ubuntu 20.04`. 
First, you should configure a `docker-compose.yml` file. 
An example for building Valhalla using Germany's tiles from OpenStreetMap can look like this:
```YML
services:
  valhalla:
    image: gisops/valhalla:latest
    ports:
      - "8002:8002"
    volumes:
      - ./custom_files/:/custom_files
    environment:
      - tile_urls = http://download.geofabrik.de/europe/germany-latest.osm.pbf
      - min_x = 5.8  # Germany’s minimum longitude
      - max_x = 15.1 # Germany’s maximum longitude
      - min_y = 47.2 # Germany’s minimum latitude
      - max_y = 55.2 # Germany’s maximum latitude
      - use_tiles_ignore_pbf = True
      - build_time_zones = True
      - build_elevation = True
      - build_admins = True
      - force_rebuild_elevation = False
      - force_rebuild = False
```
This configuration prioritize the `pbf` file located in `custom_files`. 
If this is empty, then the tiles are retrieved from the specified link from [GEOFABRIK](https://www.geofabrik.de/). 
Next, Valhalla can be built by:
```commandline
docker-compose up --build
```
When the installation is finished, Valhalla is running on `http://localhost:8002/`.








