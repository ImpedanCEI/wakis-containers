# Wakis Containers

The containers inside this repository are built and tested using `podman`. 
## Building the container
The containers can be built from the Dockerfiles depending on the desired configuration using:
```bash
podman build -t wakis-test-build ./build-target/
```
## Running the container
A bash shell running inside the container can be accessed using:
```bash
podman run --userns=keep-id -it localhost/wakis-test-build
```
The option `--userns=keep-id` is recommended to ensure that ownership of files inside mounted directories/volumes is not broken.