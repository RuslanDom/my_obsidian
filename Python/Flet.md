# Установка для Linux

```shell
sudo apt-get update
```

ВОЗМОЖНО НУЖНО ВОТ ЭТА ЗАПИСЬ

```shell
sudo apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-bad1.0-dev gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-doc gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-pulseaudio`
```

### DETEMINE VERSION (определить версию)
```shell
locate libmpv.so
```

#### IF RESULT 
```shell
/usr/lib/x86_64-linux-gnu/libmpv.so.2
/usr/lib/x86_64-linux-gnu/libmpv.so.2.2.0
```

#### THEN EXECUTE (тогда выполнить)
```shell
sudo ln -s /usr/lib/x86_64-linux-gnu/libmpv.so.2 /usr/lib/x86_64-linux-gnu/libmpv.so.1
```

```shell
sudo ln -s /usr/lib/x86_64-linux-gnu/libmpv.so.2 /usr/lib/x86_64-linux-gnu/libmpv.so.1
```