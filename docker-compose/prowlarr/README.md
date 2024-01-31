![image](https://github.com/Bratato/templates/blob/main/docker-compose/prowlarr/prowlarr-logo.png)


## Installation ##

1. Create `docker-compose.yml` (Doesn't matter where its created):
```bash
sudo nano docker-compose.yml
```

2. Copy & Paste [`docker-compose.yml`](https://github.com/Bratato/templates/blob/main/docker-compose/prowlarr/docker-compose.yml) from templates to your just created `docker-compose.yml`

3. Editing `docker-compose.yml`:

| Variable/s | Output |
| ---------- | ------ |
| `TZ` | change this to your location (i.e America/New_York) |

4. Installing Prowlarr:
```bash
sudo docker compose up -d
```

<br>

## Example ##
```yml
version: "3"

services:
  prowlarr:
    image: "lscr.io/linuxserver/prowlarr:latest"
    container_name: "prowlarr"
    restart: "unless-stopped"
    environment:
      - "PUID=1000"
      - "PGID=1000"
      - "TZ=America/New_York" # Set Timezone
    ports:
      - "9696:9696"
    volumes:
      - "/etc/docker/prowlarr/config:/config:rw"
```

<br>

### References ###
- [Prowlarr Documentation](https://github.com/linuxserver/docker-prowlarr)
