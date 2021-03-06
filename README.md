# Runing tiny sinatra application on Docker

[Alpine Sinatra](https://hub.docker.com/r/sashaegorov/alpine-sinatra/) is sample project for running a Sinatra application in Docker on top minimal Alpine Linux image.

## Usage

### Create Image

```
docker build --no-cache --force-rm --rm -t alpine-sinatra app/
```
Run in Docker
```
# Run container
$export SINARTA=$(docker run -d -p 5678:5678 alpine-sinatra)
# Check it's running
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
82d80f433583        alpine-sinatra      "foreman start -d /ap"   4 seconds ago       Up 3 seconds        0.0.0.0:5678->5678/tcp   hungry_lumiere
```
… or run locally
```
gem install rerun
rerun bundle exec foreman start
```

You can access it from your browser, [http://localhost:5678/](http://localhost:5678/).

Endpoints:
- `/env[?json=yes]` look Ma! Environment.
- `/disk` quick and dirty output of `df -h`
- `/memory` output of `free -m`
- `/exit` send TERM signal to app i.e. exit correctly
- `/fail` send KILL to app i.e. exit *incorrectly*
- `/sleep[?seconds=3.5]` wait like a pro...
- `/form` simple form with POST method

Check out logs.

```
docker logs $SINATRA
```

Stop it.

```
docker stop $SINATRA
```

Delete it.

```
docker rm $SINATRA
```

Clean up after it

```
docker images | grep '<none>' | awk '{print $3}' | xargs docker rmi -f
```

## OS X

Use Vagrant. In `Vagrantfile`, port forwarding setting included!

```
vagrant up
```

and

```
vagrant ssh
```

# Lattice
You can run this in [Lattice](http://lattice.cf)

```
ltc create --run-as-root \
--env RACK_ENV=production \
--env FOO=fubar \
--memory-mb 32 \
--monitor-command 'ps auxww | grep "rackup.*puma" | grep -v grep' \
--monitor-timeout "1s" \
--instances 2 \
alpine-sinatra sashaegorov/alpine-sinatra && \
ltc list
```

## Playing with `curl`

Here `http://localhost:5678/form` is default development URL if application was started with `rackup app/sinatra/config.ru`.

```
curl --form 'message=Hello world!✔︎' --form 'log=yes' http://localhost:5678/form
Hello world!✔︎⏎
```

## Reference

- [OSX, Vagrant, Docker, and Sinatra | DYLI.SH](http://dyli.sh/2013/08/23/OSX-Vagrant-Docker-Sinatra.html)
- [Sinatra deployment with Docker](http://haanto.com/sinatra-deployment-with-docker/)
