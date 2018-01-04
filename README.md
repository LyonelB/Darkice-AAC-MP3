# Darkice-AAC-MP3
Compilation de Darkice avec prise en charge de AAC et MP3 sur Raspberry Pi avec Raspbian Stretch

## Préparation
    $ sudo sh -c "echo 'deb-src http://mirrordirector.raspbian.org/raspbian/ stretch main contrib non-free rpi' >> /etc/apt/sources.list"
    $ sudo apt-get update
    $ sudo apt-get upgrade --yes
    $ sudo apt-get install dh-autoreconf libtool libtool-bin libasound2-dev

## Compilation de libfaac

    $ mkdir /tmp/build && cd /tmp/build
    $ apt-get -b source libfaac0 faac
    $ libtool --finish /usr/lib/arm-linux-gnueabihf
    $ sudo dpkg -i libfaac0_1.28+cvs20151130-1_armhf.deb libfaac-dev_1.28+cvs20151130-1_armhf.deb faac_1.28+cvs20151130-1_armhf.deb
  
## Compilation et installation de Darkice

    $ sudo apt-get --no-install-recommends install build-essential devscripts autotools-dev fakeroot dpkg-dev debhelper autotools-dev dh-make quilt ccache libsamplerate0-dev libpulse-dev libaudio-dev lame libjack-jackd2-dev libasound2-dev libtwolame-dev libfaad-dev libflac-dev libmp4v2-dev libshout3-dev libmp3lame-dev libopus-dev
    $ mkdir src && cd src/
    $ apt-get source darkice
    $ cd darkice-1.3/
    $ sudo nano debian/rules
  
et copiez le texte suivant : https://raw.githubusercontent.com/LyonelB/Darkice-AAC-MP3/master/debian-rules

Terminer la compilation :

    $ dpkg-buildpackage -rfakeroot -uc -b
    $ sudo dpkg -i ../darkice_1.3-0.1_armhf.deb
  
## Notes et liens 

- https://raspberrypi.stackexchange.com/a/68267
- https://archive.is/rC8We
- https://technic2radio.fr/tuto-raspberry-pi-darkice-streaming-audio/
