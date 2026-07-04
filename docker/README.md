# README

## Build 

```bash
docker build -t pvnet_clean:latest .
```

## Run

To run the docker
Add the following to your ~/.bashrc

```bash
docker build -t pvnet_clean:latest -f docker/Dockerfile . # from project root
source $PVNET_GIT/docker/setup_dev.bash
```

run it with:

```bash
docker container rm -f pvnet_dev # if previously run
pvnet_docker
```
