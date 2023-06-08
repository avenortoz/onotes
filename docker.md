## Commands Cheat sheet

- start a docker daemon
```sh
dockerd
```
- delete all containers including volumes
```sh
docker rm -vf $(docker ps -aq)
```
- delete all images
```sh
docker rmi -f $(docker images -aq)
```
- delete unused containers, images, and volumes
```sh
docker system prune -a -f
```