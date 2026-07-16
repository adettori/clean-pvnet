# README

## Build 

```bash
docker build -t pvnet_clean:latest -f docker/Dockerfile . # from project root
```

## Run

To run the docker
Add the following to your ~/.bashrc

```bash
export PVNET_DOCKER=pvnet_clean:latest
export PVNET_GIT=/path/to/pvnet/root
source docker/setup_dev.bash
```

run it with:

```bash
docker container rm -f pvnet_dev # if previously run
pvnet_docker
```
