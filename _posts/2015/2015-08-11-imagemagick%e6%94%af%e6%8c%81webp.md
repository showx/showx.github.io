---
layout: post
title: imagemagick支持webp
date: 2015-08-11 10:28
author: admin
comments: true
categories: []
---
工作需要，imagemagick要支持webp转换比较好的图片
https://gauntface.com/blog/2014/09/02/webp-support-with-imagemagick-and-php
======
cd /tmp
mkdir imagemagick
cd imagemagick
sudo apt-get build-dep imagemagick
sudo apt-get install libwebp-dev devscripts
apt-get source imagemagick
cd imagemagick-*
debuild -uc -us
sudo dpkg -i ../*magick*.deb

=============
这种可以：
wget http://webp.googlecode.com/files/libwebp-0.3.0.tar.gz
cd libwebp-0.3.0
./configure
make
sudo make install

./configure --without-perl --without-magick-plus-plus --with-webp=yes


有可能找不到 error while loading shared libraries: libwebp.so.4:库
LD_LIBRARY_PATH=/usr/local/lib convert -version
或者 cp libwebp.s* /usr/lib
