


# One-Time setup


```

apt install -y mc g++ cmake make libsigc++-2.0-dev libgsm1-dev libpopt-dev tcl tcl-dev 
apt install -y libgcrypt20-dev libspeex-dev libasound2-dev libopus-dev librtlsdr-dev 
apt install -y doxygen groff alsa-utils vorbis-tools curl libcurl4-openssl-dev libvorbis-dev
apt install -y git bc curl rtl-sdr libcurl4-openssl-dev cmake libjsoncpp-dev
apt install -y libgpiod2 libgpiod-dev libssl-dev

```


# How to quick build a project

```
mkdir src/build
cd build

cmake -DUSE_QT=OFF -DCMAKE_INSTALL_PREFIX=/usr -DSYSCONF_INSTALL_DIR=/etc -DLOCAL_STATE_DIR=/var ..
make -j4 -l2  (or just a make)
make doc
sudo make install 


```

have fun vy73 de sp0dz