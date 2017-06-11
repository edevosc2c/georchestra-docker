# geOrchestra on Docker

## Quick Start

Clone this repo to run geOrchestra on Docker in a minute:
```
git clone https://github.com/georchestra/docker.git
cd docker && git submodule update --init --remote
```

Edit your `/etc/hosts` file with the following:
```
127.0.1.1	georchestra.mydomain.org
```

Run geOrchestra with
```
docker-compose up
```

Open [https://georchestra.mydomain.org/](https://georchestra.mydomain.org/) in your browser.

To login, use these credentials:
 * `testuser` / `testuser`
 * `testadmin` / `testadmin`

To upload data into the GeoServer data volume (`geoserver_geodata`), use rsync:
```
rsync -arv -e 'ssh -p 2222' /path/to/geodata/ geoserver@georchestra.mydomain.org:/mnt/geoserver_geodata/
```
(password is: `geoserver`)

Files uploaded into this volume will also be available to the geoserver instance in `/mnt/geoserver_geodata/`.


## Notes

These docker-compose files describe:
 * which images / webapps will run,
 * how they are linked together,
 * where the configuration and data volumes are

The `docker-compose.override.yml` file adds services to interact with your geOrchestra instance (their are not part of geOrchestra "core"):
 * reverse proxy / load balancer
 * ssh / rsync services,
 * smtp, webmail.

Feel free to comment out the apps you do not need.

The base docker composition does not include any standalone geowebcache instance, nor the atlas, downloadform, catalogapp modules.
If you need them, you have to include a complementary docker-compose file at run-time:
```
docker-compose -f docker-compose.yml -f docker-compose-override.yml -f docker-compose.other-apps.yml up
```


## Customising

Adjust the configuration in the `datadir` folder according to your needs.
Reading the [quick configuration guide](https://github.com/georchestra/datadir/blob/docker-master/README.md) might help !



## Building

Images used in the current composition are pulled from docker hub, which means they've been compiled by our CI.
In case you have to build these images by yourself, please refer to the [docker images build instructions](https://github.com/georchestra/georchestra/blob/16.12/docker/README.md).
