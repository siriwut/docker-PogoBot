# Docker Pokemon Go Bot

[![Docker Stars](https://img.shields.io/docker/stars/babfrag/docker-pokemongobot.svg)](https://hub.docker.com/r/babfrag/docker-pokemongobot) 
[![Docker Pulls](https://img.shields.io/docker/pulls/babfrag/docker-pokemongobot.svg)](https://hub.docker.com/r/babfrag/docker-pokemongobot)

Docker image to run a Pokemon Go Bot from https://github.com/jabbink/PokemonGoBot (develop)

Based on [anapsix/alpine-java:8_jdk](https://hub.docker.com/r/anapsix/alpine-java)

## Install Docker

Follow [these instructions](https://docs.docker.com/engine/installation/) to get Docker running on your server.

## Tags
Two versions of image exist :
 - 1.0.0: no privilege needed, pull develop Pokemon Go Bot and run it from source
 - 1.0.0-tc: extended privileges needed to control traffic and limit requests/secondes, pull develop and run it from source

Thanks to fixes of requests spamming in bot stack, latest is now without traffic control.

## Download

Get the trusted build from the [Docker Hub registry](https://hub.docker.com/r/babfrag/docker-pokemongobot):

```
docker pull babfrag/docker-pokemongobot
```

or download and compile the source yourself from GitHub:

```
git clone https://github.com/babfrag/docker-PokemonGoBot.git
cd docker-PokemonGoBot
docker build -t babfrag/docker-pokemongobot .
```

## How to use this image

### Environment variables

This Docker image uses mandatory environment variables that override the bot [config.properties](https://raw.githubusercontent.com/jabbink/PokemonGoBot/develop/config.properties.template) defaults values, that can be declared in an `env` file (see pogobot.env.example file):

```
pogo_username=<google or ptc login>
pogo_password=<google or ptc password>
pogo_latitude=<starting latitude like 1.50>
pogo_longitude=<starting longitude like 1.50>
```

You can specify any other variables to override properties using pattern :
```
pogo_<property_to_override>=<custom value>
```

Example : if you want to override transfer_cp_threshold setting, add the following env variable:
```
pogo_transfer_cp_threshold=<your custom value>
```

Start a new Docker container with the following command (replace `./pogobot.env` with your own `env` file) :

This command uses defaults gui ports, of course you can customize them.

This container needs to run w/ privileged so the traffic control trick can be done (-d --privileged) ```tc qdisc add dev eth0 root netem delay 300ms rate 56kbit```

This avoids "RTNETLINK answers: Operation not permitted" error.

```
docker run \
    --name pogobot \
    --env-file ./pogobot.env \
    -d --privileged \
    -p 8001:8001 \
    babfrag/docker-pokemongobot
```

## docker-compose
This command uses defaults gui ports, of course you can customize them.
```
pogobot:
    image: babfrag/docker-pokemongobot
    privileged: true
    env_file:
     - <path_to_env_file>
    ports:
     - 8001:8001
    volumes:
     - /etc/localtime:/etc/localtime:ro
```

## Where did the GUI go?

The GUI is now hosted on http://pogo.abb.ink/RocketTheme/

This URL is also shown in the console when you launch the bot.


Let's catch some of them ! :sparkles::tada::rocket::sparkles:

## Author

Copyright (C) 2016 Babfrag

## All credits to [AeonLucid](https://github.com/AeonLucid), [Grover-c13](https://github.com/Grover-c13), [jabbink](https://github.com/jabbink)
