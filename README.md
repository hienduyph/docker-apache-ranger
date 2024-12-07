# Apache Ranger

## Build ranger

Prepare `.env`

```
VERSION=3.0.0
REVISION=72f6c57
```

```bash
export BUILD_USER=$(id -u) # this will make permission correctly
export M2_CACHE=$HOME/.m2 # speedup by using local m2 cache directory
export $(cat .env | xargs)
```

```bash
docker compose up
# this will took hours to complete
```

Scripts pull ranger source code into `./build-src/ranger` and target build is at `./build-src/ranger/target`
After built sucessfully, start build admin and usersync image

```bash
docker build . --build-arg VERSION=${VERSION} --build-arg REVISION=${REVISION} -t ghcr.io/hienduyph/ranger-admin:${VERSION}-${REVISION} -f ./ranger-admin/Dockerfile

docker build . --build-arg VERSION=${VERSION} --build-arg REVISION=${REVISION} -t ghcr.io/hienduyph/ranger-usersync:${VERSION}-${REVISION} -f ./ranger-usersync/Dockerfile
```

## Run samples

```bash
cd samples
docker compose up -d
```

To to ranger at `http://127.0.0.1:6080`, User and pass is `admin/rangerR0cks!`

# Notices

- `<hive.version>4.0.0</hive.version>` to works with hive jdbc
