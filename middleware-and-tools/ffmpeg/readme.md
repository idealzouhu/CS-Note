[Releases · BtbN/FFmpeg-Builds (github.com)](https://github.com/BtbN/FFmpeg-Builds/releases)



下载版本

ffmpeg-master-latest-win64-gpl.zip



配置环境变量

```
D:\Program Files\ffmpeg-master-latest-win64-gpl\bin
```



在cmd窗口里面，输入  `ffmpeg –version` 检测是否安装成功

```
C:\Users\zouhu>ffmpeg -version
ffmpeg version N-112191-g58b6c0c327-20230927 Copyright (c) 2000-2023 the FFmpeg developers
built with gcc 13.2.0 (crosstool-NG 1.25.0.232_c175b21)
configuration: --prefix=/ffbuild/prefix --pkg-config-flags=--static --pkg-config=pkg-config --cross-prefix=x86_64-w64-mingw32- --arch=x86_64 --target-os=mingw32 --enable-gpl --enable-version3 --disable-debug --disable-w32threads --enable-pthreads --enable-iconv --enable-libxml2 --enable-zlib --enable-libfreetype --enable-libfribidi --enable-gmp --enable-lzma --enable-fontconfig --enable-libharfbuzz --enable-libvorbis --enable-opencl --disable-libpulse --enable-libvmaf --disable-libxcb --disable-xlib --enable-amf --enable-libaom --enable-libaribb24 --enable-avisynth --enable-chromaprint --enable-libdav1d --enable-libdavs2 --disable-libfdk-aac --enable-ffnvcodec --enable-cuda-llvm --enable-frei0r --enable-libgme --enable-libkvazaar --enable-libass --enable-libbluray --enable-libjxl --enable-libmp3lame --enable-libopus --enable-librist --enable-libssh --enable-libtheora --enable-libvpx --enable-libwebp --enable-lv2 --enable-libvpl --enable-openal --enable-libopencore-amrnb --enable-libopencore-amrwb --enable-libopenh264 --enable-libopenjpeg --enable-libopenmpt --enable-librav1e --enable-librubberband --enable-schannel --enable-sdl2 --enable-libsoxr --enable-libsrt --enable-libsvtav1 --enable-libtwolame --enable-libuavs3d --disable-libdrm --enable-vaapi --enable-libvidstab --enable-vulkan --enable-libshaderc --enable-libplacebo --enable-libx264 --enable-libx265 --enable-libxavs2 --enable-libxvid --enable-libzimg --enable-libzvbi --extra-cflags=-DLIBTWOLAME_STATIC --extra-cxxflags= --extra-ldflags=-pthread --extra-ldexeflags= --extra-libs=-lgomp --extra-version=20230927
libavutil      58. 25.100 / 58. 25.100
libavcodec     60. 27.100 / 60. 27.100
libavformat    60. 13.100 / 60. 13.100
libavdevice    60.  2.101 / 60.  2.101
libavfilter     9. 11.100 /  9. 11.100
libswscale      7.  3.100 /  7.  3.100
libswresample   4. 11.100 /  4. 11.100
libpostproc    57.  2.100 / 57.  2.100
```



```
ffmpeg -i D:\\Dataset\\data_aishell\\wav\\test\\S0764\\BAC009S0764W0121.wav -f s16le -ac 1 -ar 16000 D:\\Dataset\\data_aishell\\16000pcm\\test\\BAC009S0764W0121.pcm
```



```
/home/zouhuang/dataset/ffmpeg -i /home/zouhuang/dataset/BAC009S0724W0121.wav -f s16le -ac 1 -ar 16000 /home/zouhuang/dataset/BAC009S0724W0121.pcm
```



```
./ffmpeg -i ./BAC009S0724W0121.wav -f s16le -ac 1 -ar 16000 ./BAC009S0724W0121.pcm
```

