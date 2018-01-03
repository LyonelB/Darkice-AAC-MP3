# Darkice-AAC-MP3
Compilation de Darkice avec prise en charge de AAC et MP3 sur Raspberry Pi avec Raspbian Jessie

## Compilation de libfaac

    $ mkdir /tmp/build && cd /tmp/build
    $ apt-get -b source libfaac0 faac
    $ sudo dpkg -i libfaac0_1.28-6_armhf.deb libfaac-dev_1.28-6_armhf.deb faac_1.28-6_armhf.deb
  
## Compilation et installation de Darkice

    $ sudo sh -c "echo 'deb-src http://mirrordirector.raspbian.org/raspbian/ jessie main contrib non-free rpi' >> /etc/apt/sources.list"
    $ sudo apt-get update
    $ sudo apt-get --no-install-recommends install build-essential devscripts autotools-dev fakeroot dpkg-dev debhelper autotools-dev dh-make quilt ccache libsamplerate0-dev libpulse-dev libaudio-dev lame libjack-jackd2-dev libasound2-dev libtwolame-dev libfaad-dev libflac-dev libmp4v2-dev libshout3-dev libmp3lame-dev libtool libtool-bin libtool-dev libasound2-dev
    $ mkdir src && cd src/
    $ apt-get source darkice
    $ cd darkice-1.2/
    $ sudo nano debian/rules
  
Modifier le texte ainsi :

    #!/usr/bin/make -f

    %:
         dh $@

    .PHONY: override_dh_auto_configure
    override_dh_auto_configure:
          ln -s /usr/share/misc/config.guess .
          ln -s /usr/share/misc/config.sub .
          dh_auto_configure -- --prefix=/usr --sysconfdir=/usr/share/doc/darkice/examples --with-vorbis-prefix=/usr/lib/arm-linux-      gnueabihf/ --with-jack-prefix=/usr/lib/arm-linux-gnueabihf/ --with-alsa-prefix=/usr/lib/arm-linux-gnueabihf/ --with-faac-prefix=/usr/lib/arm-linux-gnueabihf/ --with-aacplus-prefix=/usr/lib/arm-linux-gnueabihf/ --with-samplerate-prefix=/usr/lib/arm-linux-gnueabihf/ --with-lame-prefix=/usr/lib/arm-linux-gnueabihf/ CFLAGS='-march=armv6 -mfpu=vfp -mfloat-abi=hard'

Terminer la compilation :

    $ dpkg-buildpackage -rfakeroot -uc -b
    $ sudo dpkg -i ../darkice_1.2-0.2_armhf.deb
  
## Notes et liens 

- https://raspberrypi.stackexchange.com/a/68267
- https://archive.is/rC8We
- https://technic2radio.fr/tuto-raspberry-pi-darkice-streaming-audio/
