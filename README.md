# download_live_youtube
Deve-se instalar as ferramentas youtube-dl e ffmpeg 

### Instalando as ferramentas no Linux(Fedora) ###
#### Instalando o repositório rpmfusion ####
```console
$ sudo dnf -y install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
$ sudo dnf -y install https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

#### Instalar ffmpeg e youtube-dl ####
```console
$ sudo dnf -y install ffmpeg
$ sudo dnf -y install youtube-dl
```

### Instalando as ferramentas no Windows ###
#### Instalar Chocolatey (como administrador no powershell) ####
```console
$ Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

#### Instalar o gerenciador de pacotes Chocolatey for Windows (como administrador no powershell) ####
```console
$ choco install ffmpeg
$ choco install youtube-dl
```

### Baixando uma Live não gravada pelo Youtube ###
#### Buscando o formato/qualidade desejada ####

```console
$ youtube-dl --list-formats https://www.youtube.com/watch?v=4vYVVxotcrM
[youtube] nTZLUKbzhzI: Downloading webpage
[youtube] nTZLUKbzhzI: Downloading m3u8 information
[youtube] nTZLUKbzhzI: Downloading MPD manifest
[info] Available formats for nTZLUKbzhzI:
format code  extension  resolution note
91           mp4        256x144    HLS  269k , avc1.4d400c, 30.0fps, mp4a.40.5@ 48k
92           mp4        426x240    HLS  507k , avc1.4d4015, 30.0fps, mp4a.40.5@ 48k
93           mp4        640x360    HLS  962k , avc1.4d401e, 30.0fps, mp4a.40.2@128k
94           mp4        854x480    HLS 1282k , avc1.4d401f, 30.0fps, mp4a.40.2@128k
300          mp4        1280x720   2922k , avc1.4d4020, 60.0fps, mp4a.40.2
301          mp4        1920x1080  5552k , avc1.4d402a, 60.0fps, mp4a.40.2 (best)
```

#### Pegando a URL do vídeo do HLS m3u8 no manifesto ####
No caso foi escolhido o formato 94 da extensão .mp4 com resolução  854x480  
```console
$ youtube-dl -f 94 -g  https://www.youtube.com/watch?v=nTZLUKbzhzI
https://manifest.googlevideo.com/api/manifest/hls_playlist/expire/1587117229/ei/TSiZXoudGdjqxwTCtJLIAw/ip/177.106.178.176/id/nTZLUKbzhzI.1/itag/94/source/yt_live_broadcast/requiressl/yes/ratebypass/yes/live/1/goi/160/sgoap/gir%3Dyes%3Bitag%3D140/sgovp/gir%3Dyes%3Bitag%3D135/hls_chunk_host/r2---sn-jucj-0qpz.googlevideo.com/playlist_duration/30/manifest_duration/30/vprv/1/playlist_type/DVR/hcs/yes/initcwndbps/6660/mh/mQ/mm/44/mn/sn-jucj-0qpz/ms/lva/mv/m/mvi/1/pl/21/shardbypass/yes/dover/11/keepalive/yes/mt/1587095540/disable_polymer/true/sparams/expire,ei,ip,id,itag,source,requiressl,ratebypass,live,goi,sgoap,sgovp,playlist_duration,manifest_duration,vprv,playlist_type/sig/AJpPlLswRgIhAKhtsmWJWZq_Yo-EXtiCtvg2h0wLLumr8U9qFFwg4H69AiEAqK3L3Dwa5ZYTafg_4qcR94tqOpFkC779wY2XdUAOZaY%3D/lsparams/hls_chunk_host,hcs,initcwndbps,mh,mm,mn,ms,mv,mvi,pl,shardbypass/lsig/ALrAebAwRQIgODDoEOzWKw4Eg3a1z5SxKHx_0ZdUjYJIRM9KKXZOIDICIQCVOS-KLXt3ukniWXY4q-s1TF69T9ETyfICy8O_eKePmg%3D%3D/playlist/index.m3u8
```



#### Gravando a live completa usando url do manifesto ####

Deve rodar o seguinte comando "ffmpeg -i [url_manifest] -c copy meu_video_da_live.ts" finalizar o precesso quando a live acabar e podera reproduzir depois em qualquer player. 

```console
$ ffmpeg -i https://manifest.googlevideo.com/api/manifest/hls_playlist/expire/1587117229/ei/TSiZXoudGdjqxwTCtJLIAw/ip/177.106.178.176/id/nTZLUKbzhzI.1/itag/94/source/yt_live_broadcast/requiressl/yes/ratebypass/yes/live/1/goi/160/sgoap/gir%3Dyes%3Bitag%3D140/sgovp/gir%3Dyes%3Bitag%3D135/hls_chunk_host/r2---sn-jucj-0qpz.googlevideo.com/playlist_duration/30/manifest_duration/30/vprv/1/playlist_type/DVR/hcs/yes/initcwndbps/6660/mh/mQ/mm/44/mn/sn-jucj-0qpz/ms/lva/mv/m/mvi/1/pl/21/shardbypass/yes/dover/11/keepalive/yes/mt/1587095540/disable_polymer/true/sparams/expire,ei,ip,id,itag,source,requiressl,ratebypass,live,goi,sgoap,sgovp,playlist_duration,manifest_duration,vprv,playlist_type/sig/AJpPlLswRgIhAKhtsmWJWZq_Yo-EXtiCtvg2h0wLLumr8U9qFFwg4H69AiEAqK3L3Dwa5ZYTafg_4qcR94tqOpFkC779wY2XdUAOZaY%3D/lsparams/hls_chunk_host,hcs,initcwndbps,mh,mm,mn,ms,mv,mvi,pl,shardbypass/lsig/ALrAebAwRQIgODDoEOzWKw4Eg3a1z5SxKHx_0ZdUjYJIRM9KKXZOIDICIQCVOS-KLXt3ukniWXY4q-s1TF69T9ETyfICy8O_eKePmg%3D%3D/playlist/index.m3u8  -c copy meu_video_da_live.ts
```

