---
layout: post
title:  "[WEB][Beginner] Carve Party"
date:   2025-12-05 20:58:00 +0900
categories: wargame dreamhack
---

여섯 번째 워게임 문제이다.  
Flag 획득 하기 위해서 호박을 10,000번 클릭하여야 한다.
<br />

![Dreamhack-carve-party 메인 화면](/assets/wargame/dreamhack/images/carve-party_01.png)

브라우저로 html 파일을 열어보면 호박 이미지와 남은 클릭 수가 표시된다.  
<br />
물론 10,000번을 클릭해서 Flag를 획득하는 방법도 있겠지만,  
워게임 특성 상 좀 더 쉽게 풀 수 있는 방법이 있을 것이다.  
<br />
우선 VS Code에서 파일을 열어 코드를 살펴보았다.  
<br />

{% highlight javascript %}
$(function() {
  $('#jack-target').click(function () {
    counter += 1;
    if (counter <= 10000 && counter % 100 == 0) {
      for (var i = 0; i < pumpkin.length; i++) {
        pumpkin[i] ^= pie;
        pie = ((pie ^ 0xff) + (i * 10)) & 0xff;
      }
    }
    make();
  });
});
{% endhighlight %}

파일 제일 하단에 선언된 함수이다.  
클릭 이벤트가 발생 했을 때 실행하는 동작이 정의 되어있다.  
<br />
우리가 호박 이미지를 클릭 했을 때 일어나는 이벤트는 아래와 같다.  
1. 사용자가 화면에서 호박 이미지를 클릭하면 클릭수를 세는 counter 값이 1씩 증가한다.  
2. 클릭수가 10,000번 이하이고 100으로 나눈 나머지가 0일 때 if문 내부 동작을 실행한다.  
3. make()라는 함수를 호출한다.  
<br />

counter가 10,000번이 되기까지 2. 조건문을 총 100번 실행한다.  
조건문 내부 반복문을 실행하면 pumpkin과 pie값이 변경되는 걸 알 수 있다.  
<br />
좀 더 세부적인 내용을 분석하기 위하여 코드를 전체적으로 살펴보자.  
<br />

{% highlight javascript %}
var pumpkin = [ 124, 112, 59, 73, 167, 100, 105, 75, 59, 23, 16, 181, 165, 104, 43, 49, 118, 71, 112, 169, 43, 53 ];
var counter = 0;
var pie = 1;
{% endhighlight %}

아까 보았던 pumpkin, counter, pie 변수가 선언 되어있다.  
<br />
pumpkin은 22개의 정수로 구성된 1차원 배열이란 걸 알 수 있다.  
원소들을 보았을 때 이렇다 싶은 공통점은 없었지만,  
굳이 따지자면 pie와 XOR 연산을 진행하기 때문에 0xff(10진수 255)보다 크지 않은 수로 구성 되어있었다.  
<br />
카운팅 목적으로 사용되는 변수 counter는 0으로 초기화 하여 선언하지만,  
연산에서 사용되던 pie는 1로 초기화 하여 선언하는 것을 알 수 있다.  
<br />
<br />
문제를 풀기 위한 단서가 좀 더 필요하다.  
아래에 선언된 make() 함수를 살펴보자.  
<br />

{% highlight javascript %}
function make() {
  if (0 < counter && counter <= 1000) {
    $('#jack-nose').css('opacity', (counter) + '%');
  }
  else if (1000 < counter && counter <= 3000) {
    $('#jack-left').css('opacity', (counter - 1000) / 2 + '%');
  }
  else if (3000 < counter && counter <= 5000) {
    $('#jack-right').css('opacity', (counter - 3000) / 2 + '%');
  }
  else if (5000 < counter && counter <= 10000) {
    $('#jack-mouth').css('opacity', (counter - 5000) / 5 + '%');
  }

  if (10000 < counter) {
    $('#jack-target').addClass('tada');
    var ctx = document.querySelector("canvas").getContext("2d"),
    dashLen = 220, dashOffset = dashLen, speed = 20,
    txt = pumpkin.map(x=>String.fromCharCode(x)).join(''), x = 30, i = 0;

    ctx.font = "50px Comic Sans MS, cursive, TSCu_Comic, sans-serif"; 
    ctx.lineWidth = 5; ctx.lineJoin = "round"; ctx.globalAlpha = 2/3;
    ctx.strokeStyle = ctx.fillStyle = "#1f2f90";

    (function loop() {
      ctx.clearRect(x, 0, 60, 150);
      ctx.setLineDash([dashLen - dashOffset, dashOffset - speed]); // create a long dash mask
      dashOffset -= speed;                                         // reduce dash length
      ctx.strokeText(txt[i], x, 90);                               // stroke letter

      if (dashOffset > 0) requestAnimationFrame(loop);             // animate
      else {
        ctx.fillText(txt[i], x, 90);                               // fill final letter
        dashOffset = dashLen;                                      // prep next char
        x += ctx.measureText(txt[i++]).width + ctx.lineWidth * Math.random();
        ctx.setTransform(1, 0, 0, 1, 0, 3 * Math.random());        // random y-delta
        ctx.rotate(Math.random() * 0.005);                         // random rotation
        if (i < txt.length) requestAnimationFrame(loop);
      }
    })();
  }
  else {
    $('#clicks').text(10000 - counter);
  }
}
{% endhighlight %}

make() 함수 전체는 이렇게 구성 되어있다.  
코드를 부분적으로 잘라 살펴보자.  
<br />

{% highlight python %}
if (0 < counter && counter <= 1000) {
$('#jack-nose').css('opacity', (counter) + '%');
}
else if (1000 < counter && counter <= 3000) {
$('#jack-left').css('opacity', (counter - 1000) / 2 + '%');
}
else if (3000 < counter && counter <= 5000) {
$('#jack-right').css('opacity', (counter - 3000) / 2 + '%');
}
else if (5000 < counter && counter <= 10000) {
$('#jack-mouth').css('opacity', (counter - 5000) / 5 + '%');
}
{% endhighlight %}

우선 make() 함수에서 처음으로 실행되는 조건 분기이다.  
counter 수를 기준으로 조건 여부를 판별한다.  

css에서 opacity는 투명도를 의미한다.  
클릭수에 따라서 호박의 눈, 코, 입의 투명도를 점점 낮추는 역할을 한다.  
<br />
해당 조건을 모두 만족할 경우 아래 처럼 호박 이미지에 얼굴이 생성된다.  

![Dreamhack-carve-party 호박 얼굴](/assets/wargame/dreamhack/images/carve-party_02.png)

다만 Flag를 얻는 단서는 아니라는 것을 알 수 있다.  
이어서 코드를 분석해보자.  
<br />

{% highlight javascript %}
if (10000 < counter) {
$('#jack-target').addClass('tada');
var ctx = document.querySelector("canvas").getContext("2d"),
dashLen = 220, dashOffset = dashLen, speed = 20,
txt = pumpkin.map(x=>String.fromCharCode(x)).join(''), x = 30, i = 0;

ctx.font = "50px Comic Sans MS, cursive, TSCu_Comic, sans-serif"; 
ctx.lineWidth = 5; ctx.lineJoin = "round"; ctx.globalAlpha = 2/3;
ctx.strokeStyle = ctx.fillStyle = "#1f2f90";

(function loop() {
    ctx.clearRect(x, 0, 60, 150);
    ctx.setLineDash([dashLen - dashOffset, dashOffset - speed]); // create a long dash mask
    dashOffset -= speed;                                         // reduce dash length
    ctx.strokeText(txt[i], x, 90);                               // stroke letter

    if (dashOffset > 0) requestAnimationFrame(loop);             // animate
    else {
    ctx.fillText(txt[i], x, 90);                               // fill final letter
    dashOffset = dashLen;                                      // prep next char
    x += ctx.measureText(txt[i++]).width + ctx.lineWidth * Math.random();
    ctx.setTransform(1, 0, 0, 1, 0, 3 * Math.random());        // random y-delta
    ctx.rotate(Math.random() * 0.005);                         // random rotation
    if (i < txt.length) requestAnimationFrame(loop);
    }
})();
}
else {
$('#clicks').text(10000 - counter);
}
{% endhighlight %}

제일 상단에 위치한 조건문을 살펴보자.  
counter가 10,000번이 넘어야만 실행되는 조건문이며,  
counter가 10,000번이 넘어갈 경우 남은 클릭수를 감소 시킨다.  
<br />
긴 코드 같지만 Style 관련 항목을 제외하면,
우리에게 필요한 코드는 아래 한 줄이라고 추측 할 수 있다.  
<br />

{% highlight javascript %}
txt = pumpkin.map(x=>String.fromCharCode(x)).join(''), x = 30, i = 0;
{% endhighlight %}

pumpkin 배열에서 원소를 하나씩 추출해 join으로 이어붙인다.  
<br />
위에서 클릭 이벤트 발생 시 pumpkin과 pie의 XOR 연산을 확인했다.  
pie 변수는 비트 연산을 통해 계속 변경 된다.  
<br />
<br />

이제 Flag를 얻기 위해서 최종 pumpkin 값을 찾아내면 된다.  
브라우저의 개발자 도구 중 console탭을 활용하면,  
변수 선언 없이 그대로 pumpkin과 pie 변수를 사용할 수 있다.  
<br />
아래 반복문을 console탭에서 실행하였다.  
{% highlight javascript %}
for (let j = 0; j < 100; j++) {
    for (let i = 0; i < pumpkin.length; index++) {
        pumpkin[i] ^= pie;
        pie = ((pie ^ 0xff) + (i * 10)) & 0xff;
    }

    console.log(i, "번째", pumpkin)
}
{% endhighlight %}
혹시나 규칙성 같은게 있나 살펴보기 위해서 순서와 함께 pumpkin 값을 출력했다.  
j를 0으로 초기화 하여 0번째 ~ 99번째 총 100번을 실행하였다.  

j = 1로 선언할 경우 1번째 ~ 100번째로 콘솔에 출력되는 것을 확인 할 수 있다.  
<br />

이제 최종 pumpkin 값으로 Flag를 화면에 출력해주기 위해 counter 값을 10,001로 변경하였다.  
Flag가 출력된 최종 화면은 다음과 같다.  

![Dreamhack-carve-party Flag 획득](/assets/wargame/dreamhack/images/carve-party_03.png)

어려운 문제는 아니지만,  
코드를 읽을 줄 알아야 풀 수 있는 문제라고 생각했다.  
<br />

![Dreamhack-carve-party 문제 해결](/assets/wargame/dreamhack/images/carve-party_04.png)

보안을 시작했을 때, 운 좋게도 개발로 직무 이동을 권유 받았다.  
<br />
보안을 더 잘하고 싶어서 개발을 시작했는데,  
다시 보안을 하고 싶어 직무 이동을 하려니 이력서가 모두 개발 위주라는 생각이 들었다.  
<br />
실제로 그런 피드백을 면접에서 받았다보니,  
열심히 개발을 했었던 시간을 보답받지 못하는 느낌이었다.  
<br />
잠깐이지만, 지금껏 해왔던 개발 일을 계속 할까 싶은 고민도 들었다.  
이제 초입 단계지만 계속 워게임을 풀며 그런 불안이 조금씩 해소 되는 것 같다.  
<br />
아직 보안에 그걸 적용하지 못했을 뿐,  
내가 했던 선택이 잘못 되었던 건 아니였다고 위안 받는 것 같다.  
<br />
열심히 했던 모든 시간이 보답 받는 순간이 오기를.