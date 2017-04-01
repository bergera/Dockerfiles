# docker build -t bergera/phidgetwebservice .
# docker run -it \
#     -v /tmp/.X11-unix:/tmp/.X11-unix \ # mount the X11 socket
#     -e DISPLAY=unix$DISPLAY \
#     --device=/dev/usb \
#     --name=phidgetwebservice \
#     bergera/phidgetwebservice

FROM debian:jessie
LABEL maintainer "Andrew Berger <andrew@andrewberger.net>"

ENV WEBSERVICE_PORT=5001

EXPOSE ${WEBSERVICE_PORT}

# these files aren't actually gzipped
ADD http://www.phidgets.com/downloads/libraries/libphidget_2.1.8.20151217.tar.gz /src/libphidget_2.1.8.20151217.tar
ADD http://www.phidgets.com/downloads/libraries/phidgetwebservice_2.1.8.20151217.tar.gz /src/phidgetwebservice_2.1.8.20151217.tar

RUN apt-get update && apt-get install -y \
        build-essential \
        libusb-1.0-0-dev \
        dbus \
        # usbutils \ # uncomment for tools to debug usb devices (e.g. lsusb)
        --no-intall-recommends \
    && cd /src \
    && tar xf libphidget_2.1.8.20151217.tar \
    && cd libphidget-2.1.8.20151217 && ./configure && make && make install && cd .. \
    && tar xf phidgetwebservice_2.1.8.20151217.tar \
    && cd phidgetwebservice-2.1.8.20151217 && ./configure && make && make install && cd .. \
    && apt-get purge --auto-remove -y build-essential \
    && rm -rf /src/* \
    && rm -rf /var/lib/apt/lists/*

ENTRYPOINT ["phidgetwebservice21","-p","${WEBSERVICE_PORT}"]
