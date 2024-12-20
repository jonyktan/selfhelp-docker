# Docker Self-help
Compilation of all Docker info that I have come to rely on. To serve as a quick reference when troubleshooting issues with Docker. 



## Contents
- [Setup Docker](#setup-docker)
- [Build Docker Image](#build-docker-image)
- [Run Docker Container from Image](#run-docker-container-from-image)
- [Access a Running Container](#access-a-running-container)
- [Monitoring](#monitoring)



## Setup Docker

<details>
<summary><i>Click to expand</i></summary>

### Ubuntu

1. [Install Docker Engine](https://docs.docker.com/engine/install/ubuntu/).

1. [Remove need for `sudo` for docker](https://docs.docker.com/engine/install/linux-postinstall/). 

1. [Install NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) for GPU support. 

### Windows

<b>*Recommend to use WSL 2.</b>

1. [Install Docker Desktop](https://docs.docker.com/desktop/install/windows-install/). 

1. Enable 'Use the WSL 2 based engine' in Docker Desktop >> Settings >> General. 

1. 'Enable integration with my default WSL distro' and additional distros in Docker Desktop >> Settings >> Resources >> WSL Integration.

1. Remove need for `sudo` for docker. 

    In WSL2 shell,

    ```
    sudo groupadd docker 
    sudo usermod -aG docker $USER
    ```

1. [_OPTIONAL_] Change docker storage location in Windows. Reference: [stackoverflow](https://stackoverflow.com/a/63752264/25254222).

</details>



## Build Docker Image

Requires Dockerfile.

### Docker Build

```
docker build -f <path to dockerfile> -t <docker image name> . 
```

#### Docker Build Flags

- List of `docker build` flags: [Official Documentation](https://docs.docker.com/reference/cli/docker/buildx/build/).

### Dockerfile

- Include following snippet in Dockerfile to avoid ownership and permission issues for files/folders created within the docker container. 

    ```
    ARG USERNAME=user
    ARG USER_UID=1000
    ARG USER_GID=$USER_UID

    # Create the user
    RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME
    ```



## Run Docker Container from Image

Requires docker image.

### Docker Run

```
docker run -it <FLAGS> <image name>
```

*_Assumes user interaction with the container is required after starting the container._

### Running GUI apps from container

Reference: [stackoverflow](https://stackoverflow.com/a/75392952/25254222)

#### From WSL (i.e. Ubuntu Distro in Windows) - Works in Ubuntu app terminal or Powershell after `ubuntu` command.

```
docker run -it \
-v /tmp/.X11-unix:/tmp/.X11-unix -v /mnt/wslg:/mnt/wslg -e DISPLAY=:0 -e WAYLAND_DISPLAY=wayland-0 -e XDG_RUNTIME_DIR=/mnt/wslg/runtime-dir -e PULSE_SERVER=/mnt/wslg/PulseServer \
<image name>
```

#### From Windows

Not tested.

#### Docker Run Flags

- List of `docker run` flags: [Official Documentation](https://docs.docker.com/reference/cli/docker/container/run/).

- Enable GPU support. 

    ```
    --gpus=all
    ```

- Specify name for running container. Makes it easier to `docker exec` into the desired container.
    
    ```
    --name <container name>
    ```

- Automatically remove container after it is stopped.

    ```
    --rm
    ```

- Bind mount a volume from host to the container.

    ```
    -v <path>/<to>/<host>/<volume>:<path>/<to>/<container>/<volume>
    ```

    Alternatively,

    ```
    --mount type=bind,source=<host PATH>,target=<container PATH>
    ```

- Volume mount a volume (managed only by Docker) from host to container. Recommended for sharing data between containers. 

    ```
    --mount source=<VOLUME NAME>,target=<container PATH>
    ```

- Connect the container to a network. ([Official Documentation](https://docs.docker.com/engine/network/))

    ```
    --network <network name>
    ```

    - Default if none specified is `--network bridge`. Containers are on bridge network, isolated from host.

    - `--network host` for host and containers to be connected. 

    - `--network none` to isolate the container from all other containers and the host. 



## Access a Running Container

### Docker Exec

- Launch bash in a new terminal in a running container. "Enter" a running container.

    ```
    docker exec -it <container name> bash
    ```



## Monitoring

<details>

<summary><i>Click to expand</i></summary>

</details>
