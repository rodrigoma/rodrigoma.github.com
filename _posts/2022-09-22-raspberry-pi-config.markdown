---
layout: post
title: "Raspberry Pi Config"
tags:
- title: server
  slug: server
date:   2022-09-22 22:30:30
categories: media server
---

https://blog.anoff.io/2020-12-install-docker-raspi/

https://www.funkyton.com/install-plex-media-server-on-raspberry-pi/

https://www.funkyton.com/add-external-hard-disk-to-plex-media-library/

https://www.funkyton.com/local-mass-storage-container-with-raspberry-pi-and-external-hdd/

https://nitinmanju.medium.com/set-up-a-home-media-server-using-a-raspberry-pi-and-plex-666a44f9a3bb

https://www.ionos.com/digitalguide/server/configuration/raspberry-pi-plex/

https://raspberrytips.com/install-no-ip-raspberry-pi/

https://sharechiwai.com/post/2021-02-11-raspberry-pi-how-to-unzip-rar-unrar/

https://b2midia.freshdesk.com/support/solutions/articles/1000266606-configurando-ip-fixo-no-raspberry

```
## definir IP statico

sudo nano /etc/dhcpcd.conf

## adicionar essas linhas no arquivo
interface eth0
static ip_address=192.168.0.100/24
static routers=192.168.0.1
static domain_name_servers=192.168.0.1 8.8.8.8

#faz o reload das configuracoes
sudo nohup sh -c 'ip addr flush dev eth0 && systemctl restart dhcpcd.service' > /dev/null &
```


```
## folder para montar as HDDs
sudo mkdir /media/1TB
sudo mkdir /media/4TB
sudo mkdir /media/4TBII
sudo chown -R pi:pi /media
sudo chmod -R 755 /media


## Links Simbolicos a serem criados
ln -s /media/4TBII/CenterMediaNew CenterMediaNew
ln -s /media/4TBII/CenterMediaNew/tmp/pyload-download pyload-download
ln -s /media/4TBII/CenterMediaNew/tmp/torrents torrents-download
ln -s /media/1TB/Comics Comics
ln -s /media/1TB/Series Series
ln -s /media/4TB/SeriesAntigas SeriesAntigas


## fstab --> https://unix.stackexchange.com/questions/204641/automatically-mount-a-drive-using-etc-fstab-and-limiting-access-to-all-users-o
UUID=5BFA-041D        /media/1TB      exfat   defaults,auto,rw,uid=1000,gid=1000,umask=022,nofail 0 1
UUID=6083-157B        /media/4TB      exfat   defaults,auto,rw,uid=1000,gid=1000,umask=022,nofail 0 1
UUID=61FC-8549        /media/4TBII    exfat   defaults,auto,rw,uid=1000,gid=1000,umask=022,nofail 0 1


## [TRANSMISSION] config para rodar o script ao final do download
/config/./runScript.sh


## [PLEX] mudar user:group que executa o PLEX e o caminho do Data folder --> https://dausruddin.com/how-to-change-plex-user-running-under-in-ubuntu/
[Service]
Environment="PLEX_MEDIA_SERVER_APPLICATION_SUPPORT_DIR=/media/4TBII/CenterMediaNew/SupportApps"
User=pi
Group=pi
## caminho original do Data folder --> $PLEX_HOME/Library/Application Support/Plex Media Server/
```
