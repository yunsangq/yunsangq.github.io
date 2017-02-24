---
layout: post
title: "Ubuntu 16.04 한글 키보드 설정"
date:   2017-02-24
excerpt: "Ubuntu 16.04에서 한글 설정하는 방법에 대해 알아보자. 포스팅에서 제안하는 방법은 오른쪽 Alt키로 한/영 변환이 가능하도록 하는 방법이다."
categories: [Develop Environment]
tags: [Ubuntu]
comments: true
---

Ubuntu 16.04에서 한글 설정하는 방법에 대해 알아보자.  
포스팅에서 제안하는 방법은 오른쪽 Alt키로 한/영 변환이 가능하도록 하는 방법이다.

먼저 한글 설치방법이다.  
ibus가 아니라 fcitx를 이용한다.

* **fcitx-hangul** 설치
{% highlight bash %}
sudo apt-get install fcitx-hangul
{% endhighlight %}

>2. 키보드의 윈도우 버튼을 누르고 **System Settings**를 입력해서 실행한다. **Language Support**를 클릭하고 설치가 완료될때까지 기다린다.
3. **Language Support**에서 **Keyboard input method system**을 **fcitx**로 변경한다.
4. 재부팅한다.
5. 다시 키보드의 윈도우 버튼을 누르고 **System Settings**를 입력해서 실행한다. **Keyboard**를 클릭하고 **Shortcuts**를 클릭한다.
6. 설정되어있는 4가지 항목을 모두 **Disabled**로 바꾼다. 키를 입력해야하는 항목의 경우 키보드의 **backspace**버튼을 누르면 **Disabled**로 바뀐다.
7. **Compose Key**의 **Disabled**를 길게 눌러 **Right Alt**를 선택한다.
8. **Switch to next source**는 클릭후 키보드의 **오른쪽 Alt** 버튼을 눌러 **Multikey**로 선택한다.
9. 설정을 완료한 후 화면 상단 우측에 키보드 형태의 아이콘을 클릭한다.
10. **Configure Current Input Method**를 클릭한다.
11. **+**버튼을 눌르고 **Only Show Current Language**에 체크가 되어있다면 해제해준다.
12. 목록에서 **Hangul**을 찾아 추가해준다.
13. **Global Config** 메뉴를 찾아 클릭한다. **Trigger Input Method**를 모두 **오른쪽 Alt**키를 눌러 **Multikey**로 설정해준다.
14. **Extra key for trigger input method**는 **Disabled**로 설정하고 **Share State Among Window**를 **All**로 설정한다.
15. 재부팅하고 **오른쪽 Alt** 버튼으로 한/영 변환이 되는지 확인한다.