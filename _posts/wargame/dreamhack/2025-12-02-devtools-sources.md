---
layout: post
title:  "[WEB][Beginner] devtools-sources"
date:   2025-12-02 17:45:00 +0900
categories: wargame dreamhack
---

세 번째 워게임 문제이다.
개발자 도구 사용법을 익히기 위하여 Dream Hack에서 준비한 문제 같았다.

Flag는 개발자 도구의 Source 탭에서 찾을 수 있다는 힌트만 있었다.

![Dreamhack-devtools-sources 메인 화면](/assets/wargame/dreamhack/images/devtools-sources-01.png)

우선 VSCode에서 폴더를 열어 index.html 파일을 열었다.
Flag는 DH{} 형식이기 때문에 DH 키워드를 검색해서 Flag가 포함된 파일을 찾았다.

![Dreamhack-devtools-sources Flag 검색](/assets/wargame/dreamhack/images/devtools-sources-02.png)

DH 키워드가 포함된 파일은 하나였다.
파일에 마우스 오버를 하여 Flag 전문을 확인할 수 있었다.

![Dreamhack-devtools-sources 파일 내 Flag](/assets/wargame/dreamhack/images/devtools-sources-03.png)

원본 코드에서 줄바꿈 처리 후 DH 키워드를 검색해 Flag를 쉽게 찾을 수 있었다.

다만 권장하는 풀이 방법이 있으니 개발자 도구에서 Flag를 찾아보기로 하였다.

{% highlight php %}
"sources":["webpack:///./styles/main.scss"],
{% endhighlight %}

Flag가 포함된 파일 시작 파일에 source 정보가 기재 되어있었다.
Flag가 포함된 주석에도 WEBPACK FOOTER가 포함 되어있었다.

![Dreamhack-devtools-sources main.scss](/assets/wargame/dreamhack/images/devtools-sources-04.png)

개발자 도구에서 Source 탭을 실행하였다.
하단 webpack://을 클릭하면 styles/main.scss 파일을 쉽게 찾을 수 있었다.

해당 파일에서 Flag를 어렵지 않게 구할 수 있었다.

확실히 어려운 난이도가 아니기 때문에,
다양한 접근 방법과 도구를 사용하고자 했던 취지를 알 수 있었다.

어느순간 난이도가 높아지는 순간이 올텐데,
어서 그 순간이 왔으면 좋겠다.