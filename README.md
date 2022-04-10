# Raspberry Pi Media Server

Home media server on Raspberry Pi with Plex and rTorrent.

## Features

- Home media server with [Plex](https://www.plex.tv): Stream downloaded films, series and music to your devices. Compatible with PC, Android, iOS, Chromecast, Fire TV, smart tvs...
- Torrent server with [rTorrent](https://github.com/rakshasa/rtorrent): Download torrent files directly on the Raspberry.

## Initial setup

### Raspberry initial setup

1. Use [Raspberry Pi installer](https://www.raspberrypi.com/software/) to install raspbian (no desktop version) on an SD card.
2. Boot the Raspberry and complete the initial configuration.
3. (Optional) Use the `raspi-config` command to enable SSH.

> From this point forward you can complete the installation from the Raspberry terminal or through SSH.

### Mount external storage

1. Connect a hard-drive or SSD to the Raspberry via USB to store the media library.
2. Identify the device running `lsblk`:
     ```bash
     $ lsblk
     NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
     sda           8:0    0 931.5G  0 disk 
     └─sda1        8:1    0 931.5G  0 part /mnt/usbdrive
     ```
3. Add an entry on `/etc/fstab` to mount the device on `/mnt/media`:
     ```
     /dev/sda1       /mnt/media      ntfs    defaults        0       1
     ```
4. Reboot the Raspberry and make sure the disk is mounted on `/mnt/media/`.

### Install docker

1. Install dependencies:
     ```bash
     sudo apt-get update && sudo apt-get install -y \
          apt-transport-https \
          ca-certificates \
          curl \
          gnupg2 \
          software-properties-common \
          vim \
          fail2ban \
          ntfs-3g
     ```
2. Install GPG signatures for the docker repo
     ```bash
     curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
     sudo apt-key fingerprint 0EBFCD88
     ```
3. Add docker repo
     ```bash
     echo "deb [arch=armhf] https://download.docker.com/linux/debian \
          $(lsb_release -cs) stable" | \
     sudo tee /etc/apt/sources.list.d/docker.list
     ```
4. Add your user to the group docker
     ```bash
     sudo usermod -a -G docker $YOUR_USERNAME
     ```
5. Logout and login.

## Starting the media server

### Clone this repo on the Raspberry

Clone this repo and cd into it:

```bash
sudo apt install -y git
git clone https://github.com/manumarquezg/pi-media-server.git
cd pi-media-server
```

### Start the media server

```bash
docker-compose up -d
```

Check the Raspberry IP in the local network (`192.168.1.*`) using `ifconfig` and go to `http://<RASPBERRY_IP>/`.
