![image](https://github.com/Bratato/templates/blob/main/docker-compose/plex/Plex-Logo.png)

## Installation ##

1. Create `docker-compose.yml`:
```bash
sudo nano docker-compose.yml
```

2. Copy & Paste [`docker-compose.yml`](https://github.com/Bratato/templates/blob/main/docker-compose/plex/docker-compose.yml) from templates to your just created `docker-compose.yml`

4. Editing `docker-compose.yml` [**NOTE:** If plan on transcoding, check out [Advanced Installation](https://github.com/Bratato/templates/blob/main/docker-compose/plex/README.md#advanced-installation) before step 4]:

| Variable/s | Output |
| ---------- | ------ |
| `TZ` | change this to your location (i.e America/New_York) |
| `PLEX_CLAIM` | Connects your plex server with your plex account. [Get Token here](https://plex.tv/claim) (***NOTE: Code lasts 5 minutes***) |
| `volumes: /media` | Here you must decide where plex is installed and to point where your media is for plex. Go [here](https://github.com/Bratato/templates/tree/main/docker-compose/plex#example) for reference |
| `volumes: /transcode` | (Don't Change this) RAM Temporary Transcoding Storage | 

4. Installing Plex:
```bash
sudo docker compose up -d
```

5. Done! Now you can connect to plex by using your browser
> Example: http://[Server_IP]:32400/ (i.e http://192.168.0.23:32400/)

<br>

## Advanced Installation ##
> ⚠️**OPTIONAL**

### Nvidia Transcoding ###

#### Installation ####
1. Configuring the production respository:
```bash
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```
> (OPTIONAL) Configure with experimental packages:
```bash
sed -i -e '/experimental/ s/^#//g' /etc/apt/sources.list.d/nvidia-container-toolkit.list
```
2. Update package list:
```bash
sudo apt-get update
```
3. Installing Nvidia Container Toolkit packages:
```bash
sudo apt-get install -y nvidia-container-toolkit
```

#### Docker Integration ####
1. Configure the container runtime:
```bash
sudo nvidia-ctk runtime configure --runtime=docker
```
2. Restart Docker Daemon:
```bash
sudo systemctl restart docker
```
3. Uncomment `runtime` and **two** nvidia `environment:` variables

### Intel Transcoding ###

#### Installation ####
1. Make sure your Intel CPU supports `Intel Quick Sync`
2. Uncomment `devices:` and `- "/dev/dri:/dev/dri"`

<br>

## Example ## 
> ⚠️WARNING: Only use this example to compare to your own `docker-compose.yml`, otherwise the installation might mess up.
```yml
services:
  plex:
    container_name: plex
    image: "lscr.io/linuxserver/plex:latest"
    #! Uncomment for Nvidia Transcoding !#
#    runtime: nvidia
    network_mode: host
    ports:
      - '32400:32400'
    environment:
      PUID: '1000'
      PGID: '1000'
      TZ: 'America/New_York' # Set Timezone
      VERSION: 'docker'
      PLEX_CLAIM: 'abcdefghijk1234567890lmnopqrs' # Get Here: https://plex.tv/claim
      #! Uncomment for Nvidia Transcoding !#
#      NVIDIA_VISIBLE_DEVICES: 'all' # Variables = 'all' or '[GPU-ID]'
#      NVIDIA_DRIVER_CAPABILITIES: 'compute,utility,video'
    volumes:
      - /etc/docker/plex/config:/config:rw
      # Temp Ram Transcoding
      - /dev/shm:/transcode:rw
      # Directories where your media is stored (You can add as many volumes as you want, as long as they follow the format of the rest i.e for anime: "[/path/to/anime]:/anime"
      - /media/shows:/tvshows
      - /media/movies:/movies
      - /media/music:/music
#      - [/path/to/photos]:/photos
    #! Uncomment for Intel Transcoding !#
#    devices:
#      - /dev/dri:/dev/dri
    restart: unless-stopped
```

<br>

### References ###
- [Plex Documentation](https://github.com/linuxserver/docker-plex)
- [Nvidia Transcoding Documentation](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)
- [Intel Transcoding Documentation](https://github.com/linuxserver/docker-plex#hardware-acceleration)
