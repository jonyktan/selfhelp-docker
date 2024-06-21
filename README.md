# dockerselfhelp
Compilation of all Docker info that I have come to rely on. To serve as a quick reference when troubleshooting issues with Docker. 



## Running GUI apps from container
Reference: [stackoverflow](https://stackoverflow.com/a/75392952/25254222)

### From WSL (i.e. Ubuntu Distro in Windows)

Works in Ubuntu app terminal or Powershell after `ubuntu` command.

```
docker run -it -v /tmp/.X11-unix:/tmp/.X11-unix -v /mnt/wslg:/mnt/wslg -e DISPLAY=:0 -e WAYLAND_DISPLAY=wayland-0
-e XDG_RUNTIME_DIR=/mnt/wslg/runtime-dir -e PULSE_SERVER=/mnt/wslg/PulseServer <docker image>
```

### From Windows

Not tested.
