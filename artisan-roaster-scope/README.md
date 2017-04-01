# Artisan Dockerfile
Run [Artisan Roaster Scope](https://github.com/artisan-roaster-scope/artisan) in a docker container.

```bash
docker build -t bergera/artisan .
```

```bash
docker run -it \
    --device=/deb/bus/usb \ # se we can use USB devices
    --device=/dev/snd \ # so we can play sounds
    -e DISPLAY=unix$DISPLAY \ # X11
    -v /tmp/.X11-unix:/tmp/.X11-unix \ # X11
    -e LIBGL_DEBUG=9 \ # libgl is never very happy with Artian, log all the things!
    -v ~/artisan:/usr/share/artisan/data \ # artisan user data, like profiles
    -v ~/.artisan-settings:/usr/share/artisan/artisan-settings \ # artisan settings
    --name=artisan \
    bergera/artisan
```
