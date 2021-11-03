# NextMagentaCloud mini developer docker image



== Building the image
```
docker build -t ncdev-1.2 .
```

== Running the image
The files and directories requiring changes during development as mounted to local directories.
Set `NMC_DEV_ROOT` environment variable to the place you want to have the artifacts in.

With plain docker:
```
docker run -p 8080:80 -d \
-v $NMC_DEV_ROOT/nc_docker/apps:/var/www/html/custom_apps \
-v $NMC_DEV_ROOT/nc_docker/config:/var/www/html/config \
-v $NMC_DEV_ROOT/nc_docker/data:/var/www/html/data \
-v $NMC_DEV_ROOT/nc_docker/theme:/var/www/html/themes \
ncdev-1.2
```

With docker-compose:
```
docker-compose -d up
```

== Access by commandline

```
docker exec -ti CONTAINER_ID /bin/bash
```

