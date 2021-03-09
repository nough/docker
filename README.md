# BigBlueButton 2.3 Docker

not ready for production | [Changelog](CHANGELOG.md) | [Issues](https://github.com/bigbluebutton/docker/issues)

## Important
**This Repository - and BBB 2.3 itself - are still in an early state. Don't use it in production yet!** \
Testing and further development welcome :)

you can find a **stable bbb-docker release for 2.2** here: https://github.com/alangecker/bigbluebutton-docker


## Features
- Easy installation
- Greenlight included
- TURN server included
- Fully automated HTTPS certificates
- Full IPv6 support
- Runs on any major linux distributon (Debian, Ubuntu, CentOS,...)

## What does not work
- probably a lot - it's in an alpha state!
- bbb-lti

## Install
1. Install docker-ce & docker-compose
    1. follow instructions
        * Debian: https://docs.docker.com/engine/install/debian/
        * CentOS: https://docs.docker.com/engine/install/centos/
        * Fedora: https://docs.docker.com/engine/install/fedora/
        * Ubuntu: https://docs.docker.com/engine/install/ubuntu/
    2. Ensure docker works with `$ docker run hello-world`
    3. Install docker-compose: https://docs.docker.com/compose/install/
    4. Ensure docker-compose works and that you use a version ≥ 1.28 : `$ docker-compose --version`
2. Clone this repository
   ```sh
   $ git clone --recurse-submodules https://github.com/bigbluebutton/docker.git bbb-docker
   $ cd bbb-docker
   ```
3. Run setup:
   ```bash
   $ ./scripts/setup
   ```
4. (optional) Make additional configuration adjustments
   ```bash
   $ nano .env
   # always recreate the docker-compose.yml file after making any changes
   $ ./scripts/generate-compose
   ```
5. Start containers:
    ```bash
    $ docker-compose up -d
    ```
6. If you use greenlight, you can create an admin account with:
    ```bash
    $ docker-compose exec greenlight bundle exec rake admin:create
    ```

## Network Configuration
Services as configured.
|Service | Network | IP address | Other Option |
--- | --- | --- | --- 
| bbb-web | bbb-net | 10.7.7.2 |
| html5-backend-{{$i}} | bbb-net | 10.7.7.{{add 100 $i}}| Port {{ add 4000 $i }}
| html5-frontend-{{$i}}| bbb-net | 10.7.7.{{add 200 $i}}| Port {{ add 4100 $i }}
| freeswitch| network_mode: host | |
| nginx | network_mode: host| |    extra_hosts: <br />      - "host.docker.internal:10.7.7.1"<br />      - "core:10.7.7.2"<br />      - "etherpad:10.7.7.4"<br />      - "webrtc-sfu:10.7.7.10"<br />      - "html5:10.7.7.11"
| etherpad | bbb-net | 10.7.7.4|
| redis | bbb-net | 10.7.7.5|
| mongodb | bbb-net | 10.7.7.6|
| kurento | network-mode: host | |
| webrtc-sfu | bbb-net | 10.7.7.10|     ports:      - "127.0.0.1:3008:3008" <br />    extra_hosts:<br />      - host.docker.internal:10.7.7.1<br />      - kurento:10.7.7.1
| fsesl-akka | bbb-net | 10.7.7.14 |
| apps-akka | bbb-net | 10.7.7.15 |
| libreoffice | bbb-net | 10.7.7.7 |
| periodic | bbb-net | 10.7.7.12 |
| recordings | bbb-net | 10.7.7.16 |
| webhooks | bbb-net | 10.7.7.17 |
| https_proxy | bbb-net | |network_mode: host
| coturn | network_mode: host | |
| greenlight | | | ports: 10.7.7.1:5000:80
| prometheus | bbb-net | 10.7.7.33 |

networks:  
  bbb-net:  
    ipam:  
      driver: default  
      config:  
        - subnet: "10.7.7.0/24"


## Further How-To's
- [Upgrading](docs/upgrading.md)
- [Running behind NAT](docs/behind-nat.md)
- [BBB-Docker Development](docs/development.md)
- [Integration into an existing web server](docs/existing-web-server.md)

