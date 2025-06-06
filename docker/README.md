# openarena

## build

- docker build -t openarena-server .
- docker images
- docker run -d -p 27960:27960/udp openarena-server

On server start the following files will be created in "/data/openarena/baseoa" if they do not exist:

maps_ctf.cfg
maps_dd.cfg  
maps_dm.cfg  
maps_dom.cfg  
maps_elimination.cfg  
maps_harvester.cfg  
maps_lms.cfg  
maps_obelisk.cfg  
maps_oneflag.cfg  
maps_tdm.cfg  
maps_tourney.cfg  
server_config_sample.cfg

OA_STARTMAP - The the first map that the server loads (default oasago2)
OA_PORT - The port that the game listens on (default 27960)
OA_ROTATE_LOGS - Should the log be rotated? (default 1 = true)

If the environment OA_ROTATE_LOGS is set to "1" (witch is the default value) then "games.log" will be rotated up to once a day if the size exceeds ~50 MB. The logs will only be rotated on startup/restart. Old logs will be stored in the format "games.log.YYYY-MM-DD.gz" (this is the reason that we can only store once a day).

===========
docker pull sago007/openarena 
docker images
docker run -it -e "OA_STARTMAP=dm4ish" -e "OA_PORT=27960" --rm -p 27960:27960/udp sago007/openarena
docker ps  #id 83217db61561
docker exec -it 83217db61561 bash
docker cp 83217db61561:/data/openarena/baseoa/games.log                  ./games.log
docker cp 83217db61561:/data/openarena/baseoa/maps_ctf.cfg               ./maps_ctf.cfg
docker cp 83217db61561:/data/openarena/baseoa/maps_dd.cfg                ./maps_dd.cfg
docker cp 83217db61561:/data/openarena/baseoa/maps_dm.cfg                ./maps_dm.cfg
docker cp 83217db61561:/data/openarena/baseoa/maps_dom.cfg               ./maps_dom.cfg
docker cp 83217db61561:/data/openarena/baseoa/maps_elimination.cfg       ./maps_elimination.cfg
docker cp 83217db61561:/data/openarena/baseoa/maps_harvester.cfg         ./maps_harvester.cfg
docker cp 83217db61561:/data/openarena/baseoa/maps_lms.cfg               ./maps_lms.cfg
docker cp 83217db61561:/data/openarena/baseoa/maps_obelisk.cfg           ./maps_obelisk.cfg
docker cp 83217db61561:/data/openarena/baseoa/maps_oneflag.cfg           ./maps_oneflag.cfg
docker cp 83217db61561:/data/openarena/baseoa/maps_tdm.cfg               ./maps_tdm.cfg
docker cp 83217db61561:/data/openarena/baseoa/maps_tourney.cfg           ./maps_tourney.cfg
docker cp 83217db61561:/data/openarena/baseoa/q3config_server.cfg        ./q3config_server.cfg
docker cp 83217db61561:/data/openarena/baseoa/server_config_sample.cfg   ./server_config_sample.cfg
===========