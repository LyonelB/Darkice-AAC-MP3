# Darkice avec AAC, AAC-HE, MP3, OPUS et OGG

Compilation de Darkice avec prise en charge de AAC, AAC-HE, MP3, OPUS et OGG sur Raspberry Pi avec [Raspbian Jessie Lite](http://ftp.jaist.ac.jp/pub/raspberrypi/raspbian_lite/images/raspbian_lite-2017-07-05/)

## Préparation

    $ sudo nano /etc/apt/sources.list
    
Et supprimer le # devant deb-src (Ctrl+O pour enregistrer et Ctrl+X pour quitter)

    $ sudo apt-get update
    $ sudo apt-get upgrade
    $ sudo apt-get install dh-autoreconf libtool libtool-bin libasound2-dev libfftw3-dev build-essential devscripts autotools-dev fakeroot dpkg-dev debhelper autotools-dev dh-make quilt ccache libsamplerate0-dev libpulse-dev libaudio-dev lame libjack-jackd2-dev libasound2-dev libtwolame-dev libfaad-dev libflac-dev libmp4v2-dev libshout3-dev libmp3lame-dev libopus-dev

## Compilation de libfaac

    $ mkdir /tmp/build && cd /tmp/build
    $ apt-get -b source libfaac0 faac
    $ sudo dpkg -i libfaac0_1.28-6_armhf.deb libfaac-dev_1.28-6_armhf.deb faac_1.28-6_armhf.deb
    $ cd

## Compilation de Libaacplus

    $ mkdir src && cd src
    $ wget http://tipok.org.ua/downloads/media/aacplus/libaacplus/libaacplus-2.0.2.tar.gz
    $ tar -xzf libaacplus-2.0.2.tar.gz
    $ cd libaacplus-2.0.2
    $ ./autogen.sh --host=arm-unknown-linux-gnueabi --enable-static
    $ make
    $ sudo make install
    $ sudo ldconfig
    $ sudo reboot

## Compilation et installation de Darkice

    $ cd src
    $ apt-get source darkice
    $ cd darkice-1.2
    $ ./configure --with-faac --with-faac-prefix=/usr/lib/arm-linux-gnueabihf --with-opus --with-opus-prefix=/usr/lib/arm-linux-gnueabihf --with-pulseaudio --with-pulseaudio-prefix=/usr/lib/arm-linux-gnueabihf --with-lame --with-lame-prefix=/usr/lib/arm-linux-gnueabihf --with-alsa --with-alsa-prefix=/usr/lib/arm-linux-gnueabihf --with-jack --with-jack-prefix=/usr/lib/arm-linux-gnueabihf --with-aacplus --with-aacplus-prefix=/usr/local --with-samplerate --with-samplerate-prefix=/usr/lib/arm-linux-gnueabihf --with-vorbis --with-vorbis-prefix=/usr/lib/arm-linux-gnueabihf
    $ make
    $ sudo make install

## Configuration de Darkice

    $ cd
    $ sudo nano /etc/darkice.cfg
    
## Auto start

    $ sudo apt-get install daemontools daemontools-run
    $ sudo mkdir /etc/service/darkice
    $ sudo chmod 2775 /etc/service/darkice
    $ sudo touch /etc/service/darkice/run
    $ sudo nano /etc/service/darkice/run
    
Et ajoutez le texte suivant 

    #!/bin/sh
    echo Running service
    exec darkice

## IP fixe

    $ sudo nano  /etc/dhcpcd.conf

Et ajoutez le texte suivant 

    interface eth0
    static ip_address=192.168.1.234/24
    static routers=192.168.1.1
    static domain_name_servers=192.168.1.1
    
## Notes et liens 

- [https://github.com/rafael2k/darkice/issues/96](https://github.com/rafael2k/darkice/issues/96)
- [https://raspberrypi.stackexchange.com/a/68267](https://raspberrypi.stackexchange.com/a/68267)
- [https://archive.is/rC8We](https://archive.is/rC8We)
- [https://technic2radio.fr/tuto-raspberry-pi-darkice-streaming-audio/](https://technic2radio.fr/tuto-raspberry-pi-darkice-streaming-audio/)
- [https://github.com/coreyk/darkice-libaacplus-rpi-guide](https://github.com/coreyk/darkice-libaacplus-rpi-guide)
