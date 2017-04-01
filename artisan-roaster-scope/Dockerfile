# docker run -it \
#     --device=/deb/bus/usb \ # se we can use USB devices
#     --device=/dev/snd \ # so we can play sounds
#     -e DISPLAY=unix$DISPLAY \ # X11
#     -v /tmp/.X11-unix:/tmp/.X11-unix \ # X11
#     -e LIBGL_DEBUG=9 \ # libgl is never very happy with Artian, log all the things!
#     -v ~/artisan:/usr/share/artisan/data \ # artisan user data, like profiles
#     -v ~/.artisan-settings:/usr/share/artisan/artisan-settings \ # artisan settings
#     --name=artisan \
#     bergera/artisan
#
## TODO: figure out the udev stuff so we can run as a non-root user

FROM debian:jessie
LABEL maintainer "Andrew Berger <andrew@andrewberger.net>"

# these files aren't actually gzipped
ADD http://www.phidgets.com/downloads/libraries/libphidget_2.1.8.20151217.tar.gz /src/libphidget_2.1.8.20151217.tar
ADD https://github.com/artisan-roaster-scope/artisan/releases/download/v1.0.0/artisan-linux-1.0.0_amd64.deb /src/artisan-linux-1.0.0_amd64.deb


RUN apt-get update && apt-get install -y \
        build-essential \
        libusb-1.0-0-dev \
        xdg-utils \
        dbus \
        # usbutils \ # uncomment for tools to debug usb devices (e.g. lsusb)
    && cd /src \
    && tar xf libphidget_2.1.8.20151217.tar \
    && cd libphidget-2.1.8.20151217 && ./configure && make && make install && cd .. \
    && dbus-uuidgen > /etc/machine-id \
    && mkdir -p /usr/share/icons/hicolor \
    && dpkg -i /src/artisan-linux-1.0.0_amd64.deb \
    # && echo 'SUBSYSTEM=="usb", ATTR{idVendor}=="06c2", ATTR{idProduct}=="0032", MODE="0666"' > /etc/udev/rules.d/50-phidgettemperaturesensor.rules \
    && apt-get purge --auto-remove -y build-essential \
    && rm -rf /src/* \
    && rm -rf /var/lib/apt/lists/*

ENV QT_XKB_CONFIG_ROOT /usr/share/X11/xkb
# RUN export uid=1000 gid=1000 && \
#     mkdir -p /home/artisan && \
#     mkdir -p /etc/sudoers.d && \
#     echo "artisan:x:${uid}:${gid}:artisan,,,:/home/artisan:/bin/bash" >> /etc/passwd && \
#     echo "artisan:x:${uid}:" >> /etc/group && \
#     echo "artisan ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/artisan && \
#     usermod -aG video,audio artisan && \
#     chmod 0440 /etc/sudoers.d/artisan && \
#     chown ${uid}:${gid} -R /home/artisan
# USER artisan
# ENV HOME /home/artisan

ENTRYPOINT ["artisan"]
