---
layout: post
title:  "[WEB][Beginner] phpreg"
# date:   2025-12-05 17:35:00 +0900
categories: wargame dreamhack
---

네 번째 워게임 문제는 파일 다운로드 취약점 관련 문제이다.  

![Dreamhack-file-download-1 메인 화면](/assets/wargame/dreamhack/images/file-download-1_01.png)

flag.py 파일을 다운로드 받으면 Flag를 얻을 수 있다.  

![Dreamhack-file-download-1 업로드 화면](/assets/wargame/dreamhack/images/file-download-1_02.png)
우선 upload my memo 페이지에 접근하여 글을 작성하였다.  
<br />

![Dreamhack-file-download-1 업로드 게시글1](/assets/wargame/dreamhack/images/file-download-1_03.png)
![Dreamhack-file-download-1 업로드 게시글2](/assets/wargame/dreamhack/images/file-download-1_04.png)

내가 작성한 글은 메인 화면에서 조회 할 수 있었다.  
기능을 확인 하였으니 코드 파일을 열어보았다.  
<br />

{% highlight python %}
if filename.find('..') != -1:
    return render_template('upload_result.html', data='bad characters,,')
{% endhighlight %}

업로드 기능에서 .. 문자열에 대한 예외 처리를 확인하였다.  
path traversal를 통해 flag.py 파일을 얻으면 될 것 같다.  
<br />

![Dreamhack-file-download-1 업로드 게시글3](/assets/wargame/dreamhack/images/file-download-1_05.png)
![Dreamhack-file-download-1 업로드 게시글4](/assets/wargame/dreamhack/images/file-download-1_06.png)

../flag.py 게시글을 작성하니 코드에서 확인한 대로 bad characters 문구가 출력 되었다.  
혹시 몰라서 URL 인코딩을 적용해 동일한 게시글을 업로드 해보았다.  
<br />

![Dreamhack-file-download-1 URL 인코딩 요청](/assets/wargame/dreamhack/images/file-download-1_07.png)
![Dreamhack-file-download-1 오류 화면](/assets/wargame/dreamhack/images/file-download-1_08.png)

오류 화면이 출력되는 걸 보고 memo가 업로드 되는 동일 경로에 flag.py가 있을 거라 생각했다.  
이미 작성한 게시글에서 flag.py 파일을 호출해보았다.  
<br />

![Dreamhack-file-download-1 flag 화면](/assets/wargame/dreamhack/images/file-download-1_09.png)

예상대로 해당 경로에서 flag.py 파일을 불러오며 flag를 확인할 수 있었다.  
<br />

![Dreamhack-file-download-1 해결 화면](/assets/wargame/dreamhack/images/file-download-1_10.png)

확실히 문제에서 주어지는 단서가 조금씩 줄어드는 것 같다.  
<br />
<br />
이전 포스팅 이후 면접 때문에 포스팅이 조금 밀려버렸다.  

Beginner 단계의 문제는 풀이가 상세하지 않아도 될 것 같아서,  
한 번에 업로드 하는 편이 더 효율적이지 않을까 싶은 생각이 든다.  
<br />

다만 블로그 노출이나 관리 측면에선 하나씩 작성하는게 효율적이라,  
조금 더 고민해보고 결정을 해야할 듯 하다.  