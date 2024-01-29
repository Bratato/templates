![image](https://radarr.video/img/logo-full.png)


## Installation ##

1. Create `docker-compose.yml` (Doesn't matter where its created):
```bash
sudo nano docker-compose.yml
```

2. Copy & Paste [`docker-compose.yml`](https://github.com/Bratato/templates/blob/main/docker-compose/sonarr/docker-compose.yml) from templates to your just created `docker-compose.yml`

4. Editing `docker-compose.yml`:

| Variable/s | Output |
| ---------- | ------ |
| `TZ` | change this to your location (i.e America/New_York) |
| `volumes: /shows` | Location of TV Show library |
| `volumes: /downloads` | **(OPTIONAL)** Location of torrent downloading library |

4. Installing Radarr:
```bash
sudo docker compose up -d
```

<br>

## Example ##
```yml
version: "3"

services:
  radarr:
    image: "lscr.io/linuxserver/radarr:latest"
    container_name: "radarr"
    restart: "unless-stopped"
    environment:
      - "PUID=1000"
      - "PGID=1000"
      - "TZ=America/New_York" # Set Timezone
    ports:
      - "7878:7878" # Port Change
    volumes:
      - "/etc/docker/radarr/config:/config:rw"
      - "/etc/docker/downloads:/downloads:rw" # Optional (For automating downloading torrents)
      - "/media/server/Movies:/media/Movies:ro"
      # If you have more Movie Directories add them here:
      - "/media/server/Anime Movies:/media/AnimeMovies:ro"
```

<br>

### References ###
- [Radarr Documentation](https://github.com/linuxserver/docker-radarr)
