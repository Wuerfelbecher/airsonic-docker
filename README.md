# airsonic Docker Container based on alpine
[airsonicurl]: https://github.com/airsonic/airsonic
[![airsonic](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/airsonic-banner.png)][airsonicurl]

This is my airsonic Docker container based on alpine image with s6-overlay runs as non root by default.

## Usage

``` bash
docker run \
--rm \
--name=airsonic \
-v </path/to/data>:/airsonic \
-v </path/to/playlists>:/playlists \
-v </path/to/podcasts>:/podcasts \
-v </path/to/music>:/music \
-e AIR_GID=<gid> -e AIR_UID=<uid> \
-e AIR_CONTEXTPATH=<url-base> \
-p 4040:4040 \
-m 512m \
Wuerfelbecher/airsonic-docker

```

## Usage as systemd service

``` ini
# /etc/systemd/system/airsonic.service
# This is only a sample, please change the contents to fit your environment
[Unit]
Description=Airsonic Media Server
After=network.target local-fs.target remote-fs.target

[Service]
ExecStartPre=-/usr/bin/docker rm -f airsonic
ExecStartPre=-/usr/bin/docker pull Wuerfelbecher/airsonic-docker
ExecStart=/usr/bin/docker run \
    --rm --name=airsonic \
    -v /srv/airsonic/data:/airsonic \
    -v /srv/airsonic/playlists:/playlists \
    -v /srv/airsonic/podcasts:/podcasts \
    -v /mnt/music:/music \
    -p 4040:4040 \
    -m 512m Wuerfelbecher/airsonic-docker

[Install]
WantedBy=multi-user.target

```

## Parameters / Variables

| Parameter | Function |
| :---: | --- |
| `-p 4040` | Webserver port |
| `-v /airsonic` | Configuration file location |
| `-v /music` | Location of music |
| `-v /playlists` | Location for playlists |
| `-v /podcasts` | Location of podcasts |
| `-m 512m` | Limits the container to 512MiB RAM |

| Variable  | Function |
| :---:     | --- |
| `AIR_USR` | for custom UserName, Defaults to **airsonic**|
| `AIR_GRP` | for custom GroupName, Defaults to **airsonic**|
| `AIR_UID` | for custom UserID, Defaults to **618** |
| `AIR_GID` | for custom GroupID, Defaults to **618** |
| `CONTEXT_PATH` | for setting the url-base, Defaults to **/** |

## License

MIT License

Copyright (c) 2018 Thomas Büttner

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.