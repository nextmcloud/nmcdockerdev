# NextMagentaCloud mini developer docker image



## Building the image
```
docker build -t ncdev-1.2 .
```



## Running the image
The files and directories requiring changes during development as mounted to local directories.
Set `NMC_DEV_ROOT` environment variable to the place you want to have the artifacts in.

With plain docker the first time after build:
```
docker run --name=devnextcloud -p 8080:80 -d \
-v $NMC_DEV_ROOT/nc_docker/apps:/var/www/html/custom_apps \
-v $NMC_DEV_ROOT/nc_docker/config:/var/www/html/config \
-v $NMC_DEV_ROOT/nc_docker/data:/var/www/html/data \
-v $NMC_DEV_ROOT/nc_docker/theme:/var/www/html/themes \
ncdev-1.2
```

Next time, you only have to run:
```
docker start devnextcloud
```

If you want to reuse name for a new container version, you have to call first:
```
docker stop devnextcloud
docker rm `docker ps -a --quiet --filter name=devnextcloud`
```
Then, go ahead as described in first time run before.

TODO: With docker-compose:
```
docker-compose -d up
```

On first start, after container is up, got to http://localhost:8080
and manually finish post install procedure (e.g. set admin, install basic apps).



## Access by commandline and Nextcloud log filtering
By commandline:
```
docker exec -ti devnextcloud /bin/bash
```
The image contains composer, phpunit, php-cs-fixer and krankerl for direct use from commandline.

Filter Nextcloud log by app:
```
tail -f data/nextcloud.log |jq 'select(.app=="nmcprovisioning")'
```

Without deprecation warnings: |contains("TimedJob")
```
tail -f data/nextcloud.log |jq 'select(.app=="nmcprovisioning" and has("exception")) | select(.exception.Trace[].class|contains(["SlupCircuitHalfOpenJob", "SlupCircuitBootJob"]))'
```


## Run occ command in container
```
docker exec --user www-data devnextcloud php occ ...
```
Example 1: enable cron as Backgroundjob executor:
```
docker exec --user www-data devnextcloud php occ background:cron
```

Example 2: Config settings for OpenId connect provider
```
docker exec --user www-data devnextcloud php occ user_oidc:provider Telekom --clientid="10TVL0SAM30***000004901NEXTMAGENTACLOUD0000***" --clientsecret="F***" --bearersecret="J***" --discoveryuri="https://accounts.demo/.well-known/openid-configuration" --unique-uid="0"
```
## Enable short links
```
config.php:
  'htaccess.RewriteBase' => '/',
```
Then call:
```
docker exec --user www-data devnextcloud php occ maintenance:update:htaccess
```