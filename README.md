# Darkice-AAC-MP3
Compilation de Darkice avec prise en charge de AAC et MP3 sur Raspberry Pi avec Raspbian Jessie Lite
→ http://ftp.jaist.ac.jp/pub/raspberrypi/raspbian_lite/images/raspbian_lite-2017-07-05/

## Préparation
    $ sudo nano /etc/apt/sources.list
    
Et supprimer le # devant deb-src (Ctrl+O pour enregistrer et Ctrl+X pour quitter)

    $ sudo apt-get update
    $ sudo apt-get upgrade
    $ sudo apt-get install dh-autoreconf libtool libtool-bin libasound2-dev libfftw3-dev build-essential devscripts autotools-dev fakeroot dpkg-dev debhelper autotools-dev dh-make quilt ccache libsamplerate0-dev libpulse-dev libaudio-dev lame libjack-jackd2-dev libasound2-dev libtwolame-dev libfaad-dev libflac-dev libmp4v2-dev libshout3-dev libmp3lame-dev libopus-dev

## Compilation de libfaac

    $ mkdir /tmp/build && cd /tmp/build
    $ apt-get -b source libfaac0 faac
    $ libtool --finish /usr/lib/arm-linux-gnueabihf
    $ sudo dpkg -i libfaac0_1.28+cvs20151130-1_armhf.deb libfaac-dev_1.28+cvs20151130-1_armhf.deb faac_1.28+cvs20151130-1_armhf.deb
    
## A vérifier si nécessaire // Compilation d'Opus
    
    $ cd src
    $ wget http://downloads.xiph.org/releases/opus/opus-1.1.tar.gz
    $ tar xzvf opus-1.1.tar.gz
    $ cd opus-1.1
    $ ./configure --host=arm-unknown-linux-gnueabi
    $ make
    $ sudo make install

## Compilation de Libaacplus

    $ cd src
    $ wget http://tipok.org.ua/downloads/media/aacplus/libaacplus/libaacplus-2.0.2.tar.gz
    $ tar -xzf libaacplus-2.0.2.tar.gz
    $ cd libaacplus-2.0.2
    $ ./autogen.sh --with-parameter-expansion-string-replace-capable-shell=/bin/bash --host=arm-unknown-linux-gnueabi --enable-static
    $ make
    $ sudo make install

## Compilation et installation de Darkice

    $ mkdir src && cd src/
    $ apt-get source darkice
    $ cd darkice-1.3/

Option 1 : 
    
    $ sudo nano debian/rules
  
et copiez le texte suivant : https://raw.githubusercontent.com/LyonelB/Darkice-AAC-MP3/master/debian-rules
pour ajouter "opus", ajouter la ligne suivante :  --with-opus-prefix=/usr/lib/arm-linux-gnueabihf/ 

Terminer la compilation :

    $ dpkg-buildpackage -rfakeroot -uc -b
    $ sudo dpkg -i ../darkice_1.3-0.1_armhf.deb
  
Option 2 :

    $ ./configure --with-faac --with-faac-prefix=/usr/lib/arm-linux-gnueabihf --with-opus --with-opus-prefix=/usr/lib/arm-linux-gnueabihf --with-pulseaudio --with-pulseaudio-prefix=/usr/lib/arm-linux-gnueabihf --with-lame --with-lame-prefix=/usr/lib/arm-linux-gnueabihf --with-alsa --with-alsa-prefix=/usr/lib/arm-linux-gnueabihf --with-jack --with-jack-prefix=/usr/lib/arm-linux-gnueabihf --with-aacplus --with-aacplus-prefix=/src
    $ make
    $ sudo make install

## Configuration de Darkice

    $ cd
    $ sudo nano /etc/darkice.cfg
    
## Notes et liens 

- https://github.com/rafael2k/darkice/issues/96
- https://raspberrypi.stackexchange.com/a/68267
- https://archive.is/rC8We
- https://technic2radio.fr/tuto-raspberry-pi-darkice-streaming-audio/
- https://github.com/coreyk/darkice-libaacplus-rpi-guide
