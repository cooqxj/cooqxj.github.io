---
layout: post
title:  "[WEB][Beginner] command-injection-1"
date:   2025-12-05 18:25:00 +0900
categories: wargame dreamhack
---

다섯 번째 워게임 문제는 Command Injection 관련 문제이다.  

![Dreamhack-command-injection-1 메인 화면](/assets/wargame/dreamhack/images/command-injection-1_01.png)

ping 패킷을 보내는 서비스이다.  
flag.py 파일을 찾으면 Flag를 얻을 수 있다.  

![Dreamhack-command-injection-1 host 입력 화면](/assets/wargame/dreamhack/images/command-injection-1_02.png)
![Dreamhack-command-injection-1 ping 결과](/assets/wargame/dreamhack/images/command-injection-1_03.png)

host IP를 입력하고 ping! 버튼을 클릭해보았다.  
결과 페이지에 출력된 내용은 ping 명령어 실행 결과와 유사하다.  
<br />
입력창이 하나이기 때문에 다중 명령어를 실행하면 될 것 같았다.  
임의로 세미콜론(;) 뒤에 nslookup 명령어를 함께 입력해보았다.  
<br />

![Dreamhack-command-injection-1 에러 화면](/assets/wargame/dreamhack/images/command-injection-1_04.png)

IP가 입력되어야 하는 큰따옴표(") 내부에 세미콜론(;)과 nslookup 명령어가 함께 입력된 걸 확인할 수 있었다.  
<br />

![Dreamhack-command-injection-1 예외 처리](/assets/wargame/dreamhack/images/command-injection-1_05.png)

중간에 큰따옴표(")를 하나 더 삽입하여 명령을 실행하려고 하자,  
입력창에서 요청한 형식과 일치시켜야 한다는 문구가 출력 되었다.  
<br />
<br />

코드 파일을 열어 예외 처리 조건을 확인하였다.  

{% highlight html %}
<h1>Let's ping your host</h1><br/>
<form method="POST">
  <div class="row">
    <div class="col-md-6 form-group">
      <label for="Host">Host</label>
      <input type="text" class="form-control" id="Host" placeholder="8.8.8.8" name="host" pattern="[A-Za-z0-9.]{5,20}" required>
    </div>
  </div>

  <button type="submit" class="btn btn-default">Ping!</button>
</form>
{% endhighlight %}

글자수와 입력 가능한 문자에 대한 정규표현식이 적용 되어있었다.    
<br />
Linux Shell에서 명령어를 실행할 때, 큰따옴표(")를 사용하는 다른 명령을 함께 사용하기로 하였다.  
제일 흔하게 사용되는 echo 명령어를 함께 사용해 ls 명령어를 입력하였다.  

![Dreamhack-command-injection-1 입력 명령어](/assets/wargame/dreamhack/images/command-injection-1_06.png)
![Dreamhack-command-injection-1 실행 결과](/assets/wargame/dreamhack/images/command-injection-1_07.png)

echo 명령어로 출력한 "hi"와 함께 현재 경로 파일이 출력 되었다.  
찾아야하는 flag.py 파일이 현재 경로에 있는 것을 확인 할 수 있었다.  
<br />
Flag를 얻어야하니 cat 명령어로 flag.py 내용을 출력해보자.  
<br />

![Dreamhack-command-injection-1 flag 출력 명령어](/assets/wargame/dreamhack/images/command-injection-1_08.png)
![Dreamhack-command-injection-1 Flag 획득](/assets/wargame/dreamhack/images/command-injection-1_09.png)

어렵지 않게 Flag를 얻을 수 있었다.  
<br />

![Dreamhack-command-injection-1 해결 화면](/assets/wargame/dreamhack/images/command-injection-1_10.png)

Linux 다중 명령어 등 웹 해킹 수업 때 배운 내용들이 생각나는 문제였다.  
<br />
아직은 파일 경로를 찾아야 하거나 권한 탈취까진 요구하지 않지만,  
점점 문제 해결을 위해 요구되는 항목들이 많아지는 것 같다.  
<br />
Beginner 단계를 어서 끝내고 싶다는 마음과,  
막히는 시점이 좀 더 빨리 찾아올지도 모르겠단 걱정이 들었다.  