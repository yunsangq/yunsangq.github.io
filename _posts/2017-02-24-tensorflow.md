---
layout: post
title: "Ubuntu 16.04(64bit), Tensorflow Install"
date:   2017-02-24
excerpt: "Tensorflow CPU/GPU version 중 GPU version을 설치하는 방법에 대해 다룰 것이다. Ubuntu 16.04(64bit), CUDA 8.0, cuDNN 5.1 설치과정을 마치고나서 이 작업을 진행해야 한다.
Tensorflow의 버전은 0.12.1이며 요구하는 Requirements는 다음과 같다."
categories: [Develop Environment]
tags: [Ubuntu, Tensorflow]
comments: true
---

이 포스팅에서는 Tensorflow CPU/GPU version 중 GPU version을 설치하는 방법에 대해 다룰 것이다.  
지난 포스팅 [Ubuntu 16.04(64bit), CUDA 8.0, cuDNN 5.1 Install]에서 다루었던 Ubuntu 16.04(64bit), CUDA 8.0, cuDNN 5.1 설치과정을 마치고나서 이 작업을 진행해야 한다.  
Tensorflow의 버전은 0.12.1이며 요구하는 Requirements는 다음과 같다.

># Requirements  
The TensorFlow Python API supports Python 2.7 and Python 3.3+.  
The GPU version works best with Cuda Toolkit 8.0 and cuDNN v5.1. Other versions are supported (Cuda toolkit >= 7.0 and cuDNN >= v3) only when installing from sources.

[Ubuntu 16.04(64bit), CUDA 8.0, cuDNN 5.1 Install]을 통해 Python 2.7과 Cuda Toolkit 8.0, cuDNN v5.1의 설치를 완성해 조건을 갖춘상태로 이제 Tensorflow 설치를 시작하면된다.

{% highlight bash %}
sudo apt-get install python-pip python-dev
sudo pip install tensorflow-gpu
export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-0.12.1-cp27-none-linux_x86_64.whl
sudo pip install --upgrade $TF_BINARY_URL
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64"
export CUDA_HOME=/usr/local/cuda
{% endhighlight %}

여기까지 명령어를 수행하면 Tensorflow의 설치가 완료된다.  
테스트를 위해 다음 과정을 수행한다.  
python에서 tensorflow를 import하고 출력과 연산을 수행해본다.

{% highlight bash %}
$ python
>>> import tensorflow as tf
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
>>> print(sess.run(hello))
Hello, TensorFlow!
>>> a = tf.constant(10)
>>> b = tf.constant(32)
>>> print(sess.run(a + b))
42
{% endhighlight %}

tensorflow에서 기본으로 제공하는 예제인 MNIST demo model을 실행해본다. 숫자를 손글씨로 작성한 MNIST dataset을 이용해서 classifying handwritten digits을 수행하는 demo이다.

{% highlight bash %}
$ python -m tensorflow.models.image.mnist.convolutional
Extracting data/train-images-idx3-ubyte.gz
Extracting data/train-labels-idx1-ubyte.gz
Extracting data/t10k-images-idx3-ubyte.gz
Extracting data/t10k-labels-idx1-ubyte.gz
...etc...
{% endhighlight %}

여기까지 문제없이 진행되었다면 설치가 완료된다.

[Ubuntu 16.04(64bit), CUDA 8.0, cuDNN 5.1 Install]: http://yunsangq.github.io/articles/2017-02/Ubuntu-16.04(64bit),-CUDA-8.0,-cuDNN-5.1-Install