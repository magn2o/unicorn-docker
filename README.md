# Unofficial Docker container for [UnicornLoadBalancer](https://github.com/UnicornTranscoder/UnicornLoadBalancer)

# magn2o/unicorn-loadbalancer

I created these containers in an effort to make deploying [UnicornTranscoder](https://github.com/UnicornTranscoder) quick and painless in a docker environment. This container includes both UnicornLoadBalancer *as well as* UnicornFFMPEG. In addition to this container, you will also need at least one instance of [unicorn-transcoder](https://hub.docker.com/r/magn2o/unicorn-transcoder).

Ideally, this container should be used behind a reverse SSL proxy.

## Usage

It is strongly advised to use this within a proper [docker-compose](https://github.com/magn2o/unicorn-docker/blob/master/docker-compose.yml.template) configuration. However, should you wish to do things the hard way, please see below:

Note: You will want to ensure that you have a Plex container already configured and running before launching a load balancer.

~~~
docker run \  
-d \
--name unicorn-loadbalancer \
-p 3001:3001/tcp \
-e SERVER_HOST="127.0.0.1" \
-e SERVER_PORT="3001" \
-e SERVER_PUBLIC="http://[hostipaddress]:3001/" \
-e PLEX_HOST="[plexipaddress]" \
-e PLEX_PORT="32400" \
-e PLEX_PATH_SESSIONS="/config/Library/Application Support/Plex Media Server/Cache/Transcode/Sessions" \
-e DATABASE_SQLITE_PATH="/config/Library/Application Support/Plex Media Server/Plug-in Support/Databases/com.plexapp.plugins.library.db" \
-e LB_URL="http://[loadbalanceripaddress]:3001/" \
-h [hostname] \
-v [path]:/usr/lib/plexmediaserver \
-v [path]:/config \
-v [path]:/media:ro \
magn2o/unicorn-loadbalancer
~~~

# magn2o/unicorn-transcoder

I created these containers in an effort to make deploying [UnicornTranscoder](https://github.com/UnicornTranscoder) quick and painless in a docker environment. This container includes just UnicornTranscoder. In addition to this container, you will also need an instance of [unicorn-loadbalancer](https://hub.docker.com/r/magn2o/unicorn-loadbalancer).

One of the unique features of this container is that it will automagically identify the Plex version and necessary codec builds at runtime. This means that you can update your Plex server at will and then simplly restart this container to **maintain version parity without any manual edits**.

Ideally, this container should be used behind a reverse SSL proxy.

## Usage

~~~
docker run \  
-d \
--rm \
--name unicorn-transcoder \
-p 3000:32400/tcp \
-e SERVER_PORT="32400" \
-e LOADBALANCER_ADDRESS="http://[loadbalanceripaddress]:3001" \
-e INSTANCE_ADDRESS="http://[hostipaddress]:3000" \
-e PLEX_TOKEN="[x-plex-token]"
-h [hostname] \
magn2o/unicorn-transcoder
~~~
