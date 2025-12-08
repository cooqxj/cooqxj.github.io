---
layout: post
title:  "[WEB][Beginner] cookie"
date:   2025-12-02 17:00:00 +0900
categories: wargame dreamhack
---

처음 업로드한 문제는 DreamHack 워게임 중 Beginner 난이도의 최상단 문제였다.

Beginner 중에서도 풀이수가 많은 문제가 비교적 쉬운 난이도일 것 같아서,
풀이가 많은 문제 부터 접근하여 감을 기르기로 하였다.

![Dreamhack-cookie 초기화면](/assets/wargame/dreamhack/images/cookie-01.png)

가상화면 접근 시 처음 표시되는 화면이다.

Flag 획득 조건은 admin 계정으로 로그인 하는 것이었다.
쿠키로 인증 상태를 관리한다는 부분에서, HTTP Request 수정이 필요할 것 같아 Burp Suite를 실행해두었다.

![Dreamhack-cookie 로그인 화면](/assets/wargame/dreamhack/images/cookie-02.png)

로그인 화면에 접근하여도 별 다른 정보를 얻을 수 없었다.

Flag 조건에서 admin 계정 로그인을 제시하였기 때문에,
임의로 admin/admin 계정의 로그인을 시도하였다.

![Dreamhack-cookie 로그인 실패](/assets/wargame/dreamhack/images/cookie-03.png)

로그인 실패 시 페이지 이동이 아닌 alert가 출력 되었으며 쿠키값은 생성되지 않았다.

어제 문제 풀이 경험을 살려 바로 문제 파일을 열어 코드를 확인하였다.
파일은 app.py 하나만 제공 되어있었으며 Flask 기반으로 생성된 페이지였다.

{% highlight php %}
users = {
    'guest': 'guest',
    'admin': FLAG
}

@app.route('/')
def index():
    username = request.cookies.get('username', None)
    if username:
        return render_template('index.html', text=f'Hello {username}, {"flag is " + FLAG if username == "admin" else "you are not admin"}')
    return render_template('index.html')
{% endhighlight %}

위 코드는 app.py의 일부분이다.
코드 상에서 guest/guest 라는 계정 정보를 확인할 수 있었다.

아래 index 부분을 보면 HTTP Request 에서 Cookie값을 가져와 username에 할당하는 것을 확인할 수 있다.
Flag를 얻기 위해선 username을 admin으로 변경하면 될 것 같다.

![Dreamhack-cookie guest 로그인](/assets/wargame/dreamhack/images/cookie-04.png)

먼저 코드에서 확인된 guest 계정으로 로그인을 시도했다.

![Dreamhack-cookie username=guest](/assets/wargame/dreamhack/images/cookie-05.png)

아까와 달리 POST /login 요청을 통해 로그인을 성공하자,
페이지 리다이렉트를 위해 GET 요청이 실행되었다.

이 과정에서 Cookie 값에 username=guest가 추가된 것을 확인할 수 있었다.
메인 페이지에 Hello guest, you are not admin 이라는 문구가 추가 되었다.

![Dreamhack-cookie guest 화면](/assets/wargame/dreamhack/images/cookie-06.png)

Burp Suite를 통해 Cookie에 있는 인증 정보를 수정해보자.

![Dreamhack-cookie Cookie 수정](/assets/wargame/dreamhack/images/cookie-07.png)
![Dreamhack-cookie admin 화면](/assets/wargame/dreamhack/images/cookie-08.png)

수정한 요청을 Forward 해주니 Hello admin 문구와 함께 Flag가 출력 되었다.

예전에 들은 웹 보안 교육이 생각나는 문제였다.
Burp Suite 사용 경험 덕에 어렵지 않게 풀 수 있었다.