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