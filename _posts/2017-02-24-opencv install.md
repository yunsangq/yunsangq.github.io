---
layout: post
title: "Ubuntu 16.04(64bit), OpenCV 3.2.0 master Install"
date:   2017-02-24
excerpt: "Ubuntu에서 OpenCV를 사용하기 위한 설치방법이다. Python 2.7을 사용한다."
categories: [Develop Environment]
tags: [Ubuntu, OpenCV]
comments: true
---

Ubuntu에서 OpenCV를 사용하기 위한 설치방법이다.  
Python 2.7을 사용한다.

우선 패키지를 최신상태로 만들기 위해 업데이트를 한다.

{% highlight bash %}
sudo apt-get update
sudo apt-get upgrade
{% endhighlight %}

그리고 OpenCV 설치과정에서 필요한 각종 패키지들을 설치해준다.  
여기서 설치하는 패키지들은 이미지/비디오 파일을 읽거나 OpenCV에서 제공하는 모듈, 행렬 연산을 적용하기 위해 필요한 패키지나 라이브러리들이다.

{% highlight bash %}
# Build tools:
sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
{% endhighlight %}

OpenCV를 다운받고 build한다.

{% highlight bash %}
cd ~
git clone https://github.com/opencv/opencv.git
git clone https://github.com/opencv/opencv_contrib.git
cd opencv
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release \
-DCMAKE_INSTALL_PREFIX=/usr/local \
-DOPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
-DPYTHON2_EXECUTABLE=/usr/bin/python2.7 \
-DPYTHON_INCLUDE_DIR=/usr/include/python2.7 \
-DPYTHON_INCLUDE_DIR2=/usr/include/x86_64-linux-gnu/python2.7 \
-DPYTHON2_NUMPY_INCLUDE_DIRS=/usr/lib/python2.7/dist-packages/numpy/core/include/ ..

make -j $(($(nproc) + 1))
sudo make install
sudo ldconfig
{% endhighlight %}

설치가 완료되면 확인을 위해 아래 내용을 터미널에 입력한다.  
Python에서 OpenCV를 import해서 테스트를 한다.

{% highlight bash %}
$ python
>>> import cv2
>>> cv2.__version__
{% endhighlight %}

Github에서 clone한 master를 사용하기에 출력된 화면에 '3.2.0-dev'이라 나타나면 설치 확인을 할 수 있다.
