# Docker Self-help
Compilation of all Docker info that I have come to rely on. To serve as a quick reference when troubleshooting issues with Docker. 

## Contents
- [Running GUI apps from container](#running-gui-apps-from-container)
- [GPU Support](#gpu-support)
- [Other Useful Flags](#other-useful-flags)
- [Monitoring](#monitoring)

## Running GUI apps from container
Reference: [stackoverflow](https://stackoverflow.com/a/75392952/25254222)

<details>
<summary><i>Click to expand</i></summary>

### From WSL (i.e. Ubuntu Distro in Windows)

Works in Ubuntu app terminal or Powershell after `ubuntu` command.

```
docker run -it -v /tmp/.X11-unix:/tmp/.X11-unix -v /mnt/wslg:/mnt/wslg -e DISPLAY=:0 -e WAYLAND_DISPLAY=wayland-0 -e XDG_RUNTIME_DIR=/mnt/wslg/runtime-dir -e PULSE_SERVER=/mnt/wslg/PulseServer <docker image>
```

### From Windows

Not tested.

</details>

## GPU Support

<details>

<summary><i>Click to expand</i></summary>

Add `--gpus=all` flag in the `docker run` command. 

</details>

## Other Useful Flags

<details>

<summary><i>Click to expand</i></summary>

### Docker Build


### Docker Run


### Docker Exec

</details>

## Monitoring

<details>

<summary><i>Click to expand</i></summary>

</details>