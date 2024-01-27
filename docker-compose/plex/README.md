# Plex #

## Installation ##
1. Create `docker-compose.yml` (Doesn't matter where its created):
```bash
sudo nano docker-compose.yml
```
2. Copy & Paste [`docker-compose.yml`](https://github.com/Bratato/templates/blob/main/docker-compose/plex/docker-compose.yml) from templates to your just created `docker-compose.yml`
3. Editing `docker-compose.yml`:
- `TZ` - change this to your location (i.e America/New_York)
- `PLEX_CLAIM` - Connects your plex server with your plex account. [Get Token here](https://plex.tv/claim) (*NOTE: Code lasts 5 minutes*)
- `volumes:` - Here you must decide where plex is installed and to point where your media is for plex. Look at the example below for a reference
4. Installing Plex:
```bash
sudo docker compose up -d
```
5. Done! Now you can connect to plex by using your browser
> Example: http://[Server_IP]:32400/ (i.e http://192.168.0.23:32400/)

## Example ## 
> ⚠️WARNING: Only use this example to compare to your own `docker-compose.yml`, otherwise the installation might mess up.
```yml
version: "3"

services:
  plex:
    image: "lscr.io/linuxserver/plex:latest"
    container_name: "plex"
    #! Uncomment for Nvidia Transcoding !#
#    runtime: "nvidia"
    network_mode: "host"
    restart: "unless-stopped"
    environment:
      - "PUID=1000"
      - "PGID=1000"
      - "TZ=America/New_York" # Set Timezone
      - "VERSION=docker"
      - "PLEX_CLAIM=abcdefghijk1234567890lmnopqrs" # Get Here: https://plex.tv/claim
      #! Uncomment for Nvidia Transcoding !#
#      - "NVIDIA_VISIBLE_DEVICES=GPU-xxxxx" # Variables = 'all' or '[GPU-ID]'
#      - "NVIDIA_DRIVER_CAPABILITIES=compute,utility,video"
    ports:
      - "32400:32400"
    volumes:
      # (Note: Remove the brackets '[]')
      - "/etc/docker/plex/config:/config:rw"
      # Temp Ram Transcoding
      - "/dev/shm:/transcode:rw"
      # Directories where your media is stored (You can add as many volumes as you want, as long as they follow the format of the rest i.e for anime: "[/path/to/anime]:/anime"
      - "/media/shows:/tvshows"
      - "/media/movies:/movies"
      - "/media/music:/music"
#      - "[/path/to/photos]:/photos"
    #! Uncomment for Intel Quick-Sync Transcoding !#
#    devices:
#      - "/dev/dri:/dev/dri"
```

### References ###
> [Plex Documentation](https://github.com/linuxserver/docker-plex)
