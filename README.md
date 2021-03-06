# OpenVPN and Transmission with WebUI

[![CircleCI builds](https://img.shields.io/circleci/build/github/haugene/docker-transmission-openvpn)](https://circleci.com/gh/haugene/docker-transmission-openvpn)
[![Docker Pulls](https://img.shields.io/docker/pulls/haugene/transmission-openvpn.svg)](https://hub.docker.com/r/haugene/transmission-openvpn/)
[![Join the chat at https://gitter.im/docker-transmission-openvpn/Lobby](https://badges.gitter.im/docker-transmission-openvpn/Lobby.svg)](https://gitter.im/docker-transmission-openvpn/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

This container contains OpenVPN and Transmission with a configuration
where Transmission is running only when OpenVPN has an active tunnel.
It has built in support for many popular VPN providers to make the setup easier.

## Before you continue

The documentation for this image is here:

https://haugene.github.io/docker-transmission-openvpn/

Start there if you're having issues or questions about your container.
If you can't find your answer in the docs, please
[search for similar issues](https://github.com/haugene/docker-transmission-openvpn/issues?q=is%3Aissue+your+issue)
(open and closed) before opening a new one.

Still can't figure it out? Open a new issue and share the details of your setup and some logs.
Without that it's hard to help you. If you have a proposal for better documentation, come
with it. PR's are always welcome! :)

## Quick Start

These examples shows valid setups using PIA as provider for both
docker run and docker-compose. Note that you should read some documentation
at some point, but this is a good place to start.

### Docker run

```
$ docker run --cap-add=NET_ADMIN -d \
              -v /your/storage/path/:/data \
              -e OPENVPN_PROVIDER=PIA \
              -e OPENVPN_CONFIG=france \
              -e OPENVPN_USERNAME=user \
              -e OPENVPN_PASSWORD=pass \
              -e LOCAL_NETWORK=192.168.0.0/16 \
              --log-driver json-file \
              --log-opt max-size=10m \
              -p 9091:9091 \
              haugene/transmission-openvpn
```

### Docker Compose
```
version: '3.3'
services:
    transmission-openvpn:
        cap_add:
            - NET_ADMIN
        volumes:
            - '/your/storage/path/:/data'
        environment:
            - OPENVPN_PROVIDER=PIA
            - OPENVPN_CONFIG=france
            - OPENVPN_USERNAME=user
            - OPENVPN_PASSWORD=pass
            - LOCAL_NETWORK=192.168.0.0/16
        logging:
            driver: json-file
            options:
                max-size: 10m
        ports:
            - '9091:9091'
        image: haugene/transmission-openvpn
```

### Docker Compose with Docker secrets

Place a file called vpn_password.txt in the same directory
as the docker-compose.yaml file with a single line containing
the vpn password.

By setting appropriate permissions on this file and excluding
it from the repository, the docker-compose file can be checked
in without exposing the VPN password.

The environment variable FILE__OPENVPN_PASSWORD must contain
the name of a filename which contains the password, in this 
case /run/secrets/vpn_password is set by the [Docker secrets](https://docs.docker.com/engine/swarm/secrets/#use-secrets-in-compose)
facility.

```
version: '3.3'
services:
    transmission-openvpn:
        cap_add:
            - NET_ADMIN
        volumes:
            - '/your/storage/path/:/data'
        environment:
            - OPENVPN_PROVIDER=PIA
            - OPENVPN_CONFIG=france
            - OPENVPN_USERNAME=user
			- FILE__OPENVPN_PASSWORD=/run/secrets/vpn_password
            - LOCAL_NETWORK=192.168.0.0/16
        logging:
            driver: json-file
            options:
                max-size: 10m
        ports:
            - '9091:9091'
        image: haugene/transmission-openvpn
		secrets:
			- vpn_password
		
secrets:
	vpn_password:
		file: ./vpn_password.txt
```
## Please help out (about:maintenance)
This image was created for my own use, but sharing is caring, so it had to be open source.
It has now gotten quite popular, and that's great! But keeping it up to date, providing support, fixes
and new features takes time. If you feel that you're getting a good tool and want to support it, there are a couple of options:

A small montly amount through [![Donate with Patreon](images/patreon.png)](https://www.patreon.com/haugene) or
a one time donation with [![Donate with PayPal](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=73XHRSK65KQYC)

All donations are greatly appreciated! Another great way to contribute is of course through code.
A big thanks to everyone who has contributed so far!
