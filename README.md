# Usenet Docker (personal setup) based of the nice example from https://github.com/justinhamlett/usenet-docker

## Containers
* [plexauth](https://github.com/hjone72/PlexAuth) - Plexauth & Nginx reverse proxy (Custom docker build)
* [linuxserver/sonarr](https://github.com/linuxserver/docker-sonarr) - Sonarr
* [linuxserver/plexpy](https://github.com/linuxserver/docker-plexpy) - Plexpy
* [linuxserver/hydra](https://github.com/linuxserver/docker-hydra) - NZBHydra
* [linuxserver/radarr](https://github.com/linuxserver/docker-radarr) - Radarr
* [linuxserver/nzbget](https://github.com/linuxserver/docker-nzbget) - NZBGet
* [plexinc/pms-docker](https://github.com/plexinc/pms-docker) - Plex (Official docker image)
* [portainer/portainer](https://github.com/portainer/portainer) - Portainer
* [steynovich/spotweb](https://github.com/steynovich/docker-spotweb) - Spotweb
* [mariadb](https://hub.docker.com/_/mariadb/) - Database (mariadb)

## Custom fitting
* Things need tweaking and won't work out of the box. You need to adjust a lot of settings here and there. Starting with the .env file in this folder. After that go through the storage/config folders and read inside those folders to see if you need to make changes to make it all work.

## Docker Setup
1. Update `./.env` with the user and group IDs that will be running Docker
2. Update `./.env` and replace CONFIG and DATA paths to fit your needs (data being your collection, configs where all the tools will write their magic)
3. Add remove the containers you want, portainer is something extra to see what still runs and not (i find it handy)
4. The ssl in the nginx is a self signed cert, found in plexauth/root/ folder. Either patch it up with encrypt (PR?) or be lazy and add cloudflare on top of it (and set it to full ssl, instead of strict)
5. Edit the example folder where all your configs are going to be, mainly only plexauth needs editing. You can find your .inc files and nginx information you need to alter to make it work. Based of nginx example 1 from [plexauth](https://github.com/hjone72/PlexAuth/tree/master/nginx_example)
3. Run `docker-compose up -d` to start all the Usenet services

## Database
* A database is in use for plexauth and spotweb, once started the spotweb one will be created automaticly. Plexauth permissions should be done too, if not run the .sql from plexauth config folder. If you want to change the passwords you have to update the .env and the sql file. Database only allows localhost and the 172.x.x.x private range.

## Spotweb
* An extra script is added in the root of this folder called "spotwebcustom.sh" the only thing it does is change some templates so that spots with more then 30 comments get marked as "green" so you can see them easier.

## Domains
* The idea is to have all containers under neath / so that /radarr/ and /nzbget/ works. In my config i changed the base paths of the tools and added that too. In theory you don't have too but its better for adoptability.
* You only need port 443 to be open, 80 could be open and have a redirect to 443. 

## PlexAuth
* This setup is using nginx with plexauth. You authenticate with plexauth and plexauth will be the security to everything else. You could even remove all the auths from the other containers because it is enforced by the urls.
* BEWARE! This is done inside a docker compose, so you have your own network inside docker to talk to eachother. That way only port 80/443 will be exposed in that network. If you open up ports directly to the other containers people can and will connect to it.

## PlexAuth Permissions
* Change the G_CAPTCHA in your .env by getting one from google recaptcha website with your domain, otherwise the captcha won't work.

## Overal setup
* When all is running, you have to link nzbhydra to sonarr/radarr and same goes for ombi to point to it (don't forget in ombi to set external url and basepath) 

Since most is a combination that start with PlexAuth, you can also find me there if you need a hand: https://discord.gg/dWcjsv6
