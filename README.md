# James_S60 - Docker OpenTTD 

[Dockerhub](https://hub.docker.com/r/jamess60/openttd)

## Intro

Whilst there are a couple of decent pre-existing OpenTTD containers, I opted to make my own due to a few bugs/issues I had experienced with them. 
My approach is to build OpenTTD from source (as opposed to using the official binaries) - this allows the container to run natively on any docker/podman supported architecture. The container is a minimal configuration approach to the server in which the user is expected to perform manual configuration. 


## How does it compare to the competition?

The [bateau84/openttd](https://github.com/bateau84/openttd) appears to be the community default (based on number of pulls). My container differs in the following ways: 
- It builds OpenTTD from source, so it can be built on any architecture (not just x86) - This has a huge performance boost for aarch64
- It's arguably lighter weight - The use of SH scripts and the execution ordering goes against docker image layer philosphy, so the (cleanup)[https://github.com/bateau84/openttd/blob/master/cleanup.sh] could make the image larger! (While I appreciate my image is large, this is because source is left over due to dynamic/static linking etc. Will explore tidying up the compile flags later.)
- More up to date packages/openttd version. At time of writing, this image still uses OTTD 13.x


The [hauschi86/openttd-server (aka andreashauschild)](https://github.com/andreashauschild/openttd-server) is a nice option because of its fancy web gui, allowing you to host multiple OTTD servers from one container. While using this container, I experienced both performance issues (clients dropping due to cant keep up with server errors, even over LAN on powerful systems) and several [map corruption/deletion problems](https://github.com/andreashauschild/openttd-server/issues/16). Another user opened a [potentially similar issue](https://github.com/andreashauschild/openttd-server/issues/8) in 2023 and did not get a satisfactory reply. It appears this project is inactive (aside from TTD version updates and a recent gui tweak). In addition, this image is still x86 only as opposed to my multiarch. 


## How to deploy/use

1. Add the following to your docker compose:

   ```yaml
    services:
   
        openttd:
            container_name: openttd
            image: jamess60/openttd:latest
            restart: unless-stopped
            ports:
               - 3979:3979
            volumes:
                - /path/to/config:/root/.local/share/openttd/config
                - /path/to/saves:/root/.local/share/openttd/save/
   
   ```

or use docker run. Example: ```docker run -v ./save:/root/.local/share/openttd/save/ -v ./config:/root/.config/openttd/ -p 3979:3979 -it james_s60/openttd:latest```

2. Run the server once (to populate the config dir) and then exit 

3. Edit the openttd.cfg file to your liking. An example file is provided, but you should refer to the [official documentation](https://wiki.openttd.org/en/Archive/Manual/Settings/Openttd.cfg) for a full list of parameters. You can also generate the file via the OpenTTD client gui options menu. There are no environment variables for this container. 

4. Launch the container again and start playing :) 


### Save/Load game behaviour
The default behaviour is to generate a new game (based on the openttd.cfg provided) every time the container restarts. To load a game, type ```load filename.sav``` (see the save directory for list of save games). To save a game manually, type ```save filename``` (.sav is automatically appended). The server will also autosave at the frequency specified in openttd.cfg. You can start a new game by issueing ```new_game```



## Other 
- Bugs or feature suggestions? - Please raise a git issue :) 
- Owner of one of the aforementioned containers and believe my evaluation is inaccurate? Firstly, apologies. Please reach out to me :) 




