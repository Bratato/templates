![image](https://github.com/Bratato/templates/blob/main/docker-compose/prowlarr/Prowlarr-Logo.png)


## Installation ##

1. Create `docker-compose.yml`:
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
services:
  prowlarr:
    container_name: prowlarr
    image: lscr.io/linuxserver/prowlarr:latest
    ports:
      - '9696:9696'
    environment:
      PUID: '1000'
      PGID: '1000'
      TZ: 'America/New_York' # Set Timezone
    volumes:
      - /etc/docker/prowlarr/config:/config:rw
    restart: unless-stopped
```

<br>

### References ###
- [Prowlarr Documentation](https://github.com/linuxserver/docker-prowlarr)
