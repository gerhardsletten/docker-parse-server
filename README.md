# docker-parse-server

[![Join the chat at https://gitter.im/yongjhih/docker-parse-server](https://badges.gitter.im/yongjhih/docker-parse-server.svg)](https://gitter.im/yongjhih/docker-parse-server?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[![](https://badge.imagelayers.io/yongjhih/parse-server:latest.svg)](https://imagelayers.io/?images=yongjhih/parse-server:latest)

![](art/parse-rip-32dp.png)

Welcome PR.

Here is overview:

![](art/chart.png)

## Usage

```sh
docker run -d -p 27017:27017 --name mongo mongo
docker run -d -e APP_ID={appId} -e MASTER_KEY={masterKey} -p 1337:1337 --link mongo yongjhih/parse-server
```

* api: localhost:1337
* mongodb: localhost:27017

or with docker-compose:

```sh
wget https://github.com/yongjhih/docker-parse-server/blob/master/docker-compose.yml
APP_ID=myAppId MASTER_KEY=myMasterKey docker-compose up
```

### Usage of already mongodb with DATABASE_URI

```sh
docker run -d -e DATABASE_URI={mongodb://mongodb.intra:27017/dev} APP_ID={appId} -e MASTER_KEY={masterKey} -p 1337:1337 --link mongo yongjhih/parse-server
```

or with docker-compose:

```sh
wget https://github.com/yongjhih/docker-parse-server/blob/master/docker-compose.yml
DATABASE_URI={mongodb://mongodb.intra:27017/dev} APP_ID=myAppId MASTER_KEY=myMasterKey docker-compose up
```

### Usage of already parse-cloud-code

With host folder:

```sh
docker run -d -v /home/yongjhih/parse/cloud:/parse/cloud -e DATABASE_URI={mongodb://mongodb.intra:27017/dev} APP_ID={appId} -e MASTER_KEY={masterKey} -p 1337:1337 --link mongo yongjhih/parse-server
```

With volume container:

```sh
docker create -v /parse/cloud --name parse-cloud-code yongjhih/parse-cloud-code echo ls /parse/cloud
docker run -d --volumes-from parse-cloud-code -e DATABASE_URI={mongodb://mongodb.intra:27017/dev} APP_ID={appId} -e MASTER_KEY={masterKey} -p 1337:1337 --link mongo yongjhih/parse-server
```

### Integration of parse-cloud-code image on GitHub and DockerHub

1. fork https://github.com/yongjhih/parse-cloud-code
2. Create an automated build image on DockerHub for forked parse-cloud-code repository
3. `docker pull {username}/parse-cloud-code`
4. re/create volume container: `docker create ...`
5. re/create parse-server container: `docker run -d --volumes-from ...`

## More configuration with docker

* Specify application ID: `-e APP_ID`
* Specify master key: `-e MASTER_KEY=`
* Specify database uri: `-e DATABASE_URI=mongodb://mongodb.intra:27017/dev`
* Specify parse-server port on host: `-p 1338:1337`
* Specify database port on host: `-p 27018:27017`
* Specify parse cloud code host folder: `-v /home/yongjhih/parse/cloud:/parse/cloud`
* Specify parse cloud code volume container: `--volumes-from parse-cloud-code`
* Specify parse-server prefix: `-e MOUNT_PATH=/parse`

## Configuration with docker-compose.yml

Environment:

```yml
# ...
parse-server:
  # ...
  environment:
    DATABASE_URI: $DATABASE_URI
    APP_ID: $APP_ID
    MASTER_KEY: $MASTER_KEY
    MOUNT_PATH: $MOUNT_PATH
# ...
```

Remote parse-cloud-code image:

```yml
# ...
parse-cloud-code:
  # ...
  image: yongjhih/parse-cloud-code # Specify your parse-cloud-code image
# ...
```

or host folder:

```yml
# ...
parse-cloud-code:
  # ...
  image: yongjhih/parse-server
  volumes:
    - /home/yongjhih/parse/cloud:/parse/cloud
  # ...
# ...
```

## See Also

* https://github.com/ParsePlatform/parse-server
* http://blog.parse.com/announcements/introducing-parse-server-and-the-database-migration-tool/
* https://parse.com/docs/server/guide#migrating
* https://hub.docker.com/r/yongjhih/parse-server/
* https://github.com/yongjhih/parse-cloud-code
* https://hub.docker.com/r/yongjhih/parse-cloud-code/
* https://medium.com/cowbear-coder/migration-of-parse-server-with-docker-part1-87034cc29978
