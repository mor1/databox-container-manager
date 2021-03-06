# Databox Container Manager

Databox OS container manager and dashboard user interface.

## Installation

All Databox components, including the container manager, run in Docker
containers. So
first, [install and run Docker](https://www.docker.com/products/docker).

### Known issues:

In some cases, the time in Docker containers on Mac OSX can get out of sync with
the system clock. This causes the HTTPS certs generated by the CM from being
valid. See <https://github.com/docker/for-mac/issues/17>. Fix: restart
Docker-for-Mac.

## Running

Once docker is installed, run the following to get your Databox up and
running:

```
docker run                                          \
       -v /var/run/docker.sock:/var/run/docker.sock \
       --name databox-cm                            \
       --label databox.type=container-manager       \
       -p 8989:8989                                 \
       -it databoxsystems/databox-cm
```

If you receive an error of the form

```
docker: Error response from daemon: Conflict. The container name "/databox-cm"
is already in use by container <foo>. You have to remove (or rename) that container to be able to reuse that name..
See 'docker run --help'.
```

...then first

```
docker stop databox-cm
docker rm databox-cm
```

...and try again. Alternatively, just run the `startDatabox.sh`
script [here](https://github.com/me-box/databox/blob/master/startDatabox.sh).

Once it's started, browse to <http://127.0.0.1:8989> and have fun! This is
Databox's normal mode of operation, using an external app store and image
repository for apps.

## Development

To develop for the Databox platform we suggest running the platform in _dev
mode_. This will run a local app store and image repository in containers on
your machine. In this mode it is possible to build and replace any part of the
platform.

First clone [this repository](https://github.com/me-box/databox):

    git clone https://github.com/me-box/databox.git
    cd databox
    npm install

Then launch in dev mode by executing `sudo ./platformDevMode.sh`. A new
container will be launched, and additional instructions will be presented.

NB: Mount `./certs` and `./slaStore` as volumes if you want TLS certs and
launched apps to save between restarts.

To test a Databox app, follow app dev documentation to build it, then push the
app image to the local registry that is launched automatically in platform dev
mode (<http://localhost:5000/>). The app can then be launched normally through
the dashboard.

### ENV VARS

- `DATABOX_DEV=1` enables platform dev mode
- `DATABOX_SDK=1` enable cloud SDK mode
- `PORT=8081` overrides default port (8989)
