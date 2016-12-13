# Docker HTPC

This repo attempts to be a starting point for rolling out a Plex/Sonarr/Sabnzdb/Couchpotato media server
along with a simple portal interface and reverse proxy server.

## Setup

1. Substitute all the variables in `docker-compose` with your information.

2. To set up all the containers, just run:

```sh
docker-compose up -d
```

You should now be able to:
  - access the static portal at http://localhost.
  - access plex at http://localhost:32400/web

If there did not exist configuration files on the Docker host's mounted volumes, `/mnt/nas/htpc_config`
  - Sabnzdb will greet you with a wizard for setup
  - Sonarr will be a blank page with the text `Sonarr ver.`. [Fix it](#fixing-sonarr)
  - Couchpotato will appear to do nothing. [Fix it](#fixing-couchpotato)


### Fixing Sonarr

Sonarr needs a base url configured before it will server its web interface. The reverse proxy definitions
state that `/sonarr` points to the Sonarr url so we need to set the base url in the config file.

Edit the config file inside of the docker container.

```sh
docker exec -it sonarr sh
vi /config/.config/NzbDrone/config.xml
# enter '/sonarr' between the <UrlBase> tags
exit

#restart the container
docker-compose restart sonarr
```
### Fixing Couchpotato

CP also need and base path configured. 

```sh
docker exec -it couchpotato sh
vi /config/.config/Couchpotato/config.ini
# find 'base path' setting and enter '/couchpotato'
exit

#restart the container
docker-compose restart sonarr
```
