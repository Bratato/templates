![image](https://raw.githubusercontent.com/Bratato/templates/main/docker-compose/sonarr/sonarr-logo.png)


## Installation ##

1. Create `docker-compose.yml`:
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

4. Installing Sonarr:
```bash
sudo docker compose up -d
```

<br>

## Example ##
```yml
services:
  sonarr:
    container_name: sonarr
    image: lscr.io/linuxserver/sonarr:latest
    ports:
      - '8989:8989'
    environment:
      PUID: '1000'
      PGID: '1000'
      TZ: 'America/New_York' # Set Timezone
    volumes:
      - /etc/docker/sonarr/config:/config:rw
      - /etc/docker/downloads:/downloads:rw # Optional (For automating downloading torrents)
      - /media/shows:/media/Shows:ro
      # If you have more TV Show Directories add them here:
      - /media/animeshows:/media/animeshows:ro
    restart: unless-stopped
```

<br>

### References ###
- [Sonarr Documentation](https://github.com/linuxserver/docker-sonarr)
