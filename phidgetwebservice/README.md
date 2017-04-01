# Phidget Web Service Dockerfile
Run the [Phidget Web Service](http://www.phidgets.com/docs/Phidget_WebService) in a docker container.

```bash
docker build -t bergera/phidgetwebservice .
```

```bash
docker run -it \
    -v /tmp/.X11-unix:/tmp/.X11-unix \ # mount the X11 socket
    -e DISPLAY=unix$DISPLAY \
    --device=/dev/usb \
    --name=phidgetwebservice \
    bergera/phidgetwebservice
```
