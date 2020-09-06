---
# Common-Defined params
title: "Rtools를 윈도우 환경변수 PATH에 추가하는 방법"
date: "2020-09-06T10:00:00+09:00"
description: "How to add Rtools to windows PATH environment variable?"
categories:
  - "R"
tags:
  - "Rcpp"
menu: side # Optional, add page to a menu. Options: main, side, footer

# Theme-Defined params
#thumbnail: "/posts/Variational-Inference/week6_VI_hun-01.jpg" # Thumbnail image
lead: "How to add Rtools to windows PATH environment variable?" # Lead text
comments: true # Enable Disqus comments for specific page
authorbox: true # Enable authorbox for specific page
pager: true # Enable pager navigation (prev/next) for specific page
toc: false # Enable Table of Contents for specific page
mathjax: true # Enable MathJax for specific page
sidebar: "right" # Enable sidebar (on the right side) per page
widgets: # Enable sidebar widgets in given order per page
  - "search"
  - "recent"
  - "taglist"
---



윈도우에서 리눅스 기반으로 만들어진 오픈소스 프로그램을 사용하면 필사적인 구글링을 해야하는 문제가 정말 주기적으로 발생하는 것 같습니다. Rmarkdown으로 Rcpp 함수를 쓸 때 knit가 안 되어, 해결하는데 꽤 고생했습니다. 혹시 이런 문제를 겪으실 분을 위해 간단한 가이드를 작성했습니다. 도움이 됐으면 좋겠습니다!

Using opensource software such as R in windows is to face a constant barrage of quirky weird errors and to desperately search for help. Unexceptionally, Rcpp package kindly gave me another of such headache. I made a short guide on adding Rtools to system path, as it seemed to be the root of all evil related to Rcpp induced problems. Hope it helps!

### <br>왜 해야 하는가? (Why bother?)

Rcpp 패키지에서, 예컨대 cppFunction을 Rmarkdown으로 쓰고 knit할 때 제대로 knit가 안 되는 경우가 있다. 주로 이딴 경고를 내뱉는다.

Using Rcpp packages, when trying to knit ``.Rmd`` file containing functions from Rcpp (in my case cppFunction()), you might from time to time face the following error;

```
Error in (function (file = "", code = NULL, env = globalenv(), embeddedR = TRUE,  : 
  Error 1 occurred building shared library.
Calls: <Anonymous> ... block_exec -> in_dir -> engine -> do.call -> <Anonymous>
```

이런 메세지가 발생하는 이유는 시스템 경로에 R tools가 설치된 경로가 포함되지 않았기 때문이다. 직접 R 콘솔에 ```Sys.getenv()['PATH']```를 쳐서 확인해보자.

This pops up because for some reason, even if you have Rtools installed in your system, the installed path is NOT in the windows ```PATH``` environment variable.

골때리는거는 분명히 이런 에러를 내뱉는 데도 불구하고 ```devtools::find_rtools()``` 함수를 치면 몹시 천연덕스럽게 "TRUE^^"를 뱉는 훼이크를 시전한다는 것. 때문에 인위적으로 Rtools가 설치된 경로를 시스템 경로에 추가해야 한다.

One pervert thing about this error is that even if you type ```devtools::find_rtools()```, which asks the console whether you found Rtools, the console would still answer "TRUE", which is clearly a false positive. Hence we need to manually add Rtools path to windows ```PATH``` environment variable.

그 전에 몇 가지 알고 넘어가자.

Before that, let us see what an envrionment variable is.



### <br>환경변수가 뭐냐? What is the environment variable?

환경변수란 운영체제나 사용자 정보 등 "환경"에 관한 모든 변수들을 말한다. 프로그램을 컴푸터에 깔면 이 프로그램은 여기가 어디지? 어디로 가야하지? 라는 정보를 이 환경변수라는 것들을 보면서 파악한다고 보면 될 것이다. 환경변수를 보는 방법은, cmd 창을 켜고 그냥 env라고 누르면 시꺼먼 창에 무언가 주루룩 나오는데, 걔네들이 다 환경변수이다. 전부 다 ``APPDATA=C:\Users\gyeon\AppData\Roaming`` 이런 식으로 변수=값 형식으로 되어있다. 여기서 주목할 변수는 바로 ``PATH``이다.

Environment variable refers to a set of all the variables that have information about the "environment" of your PC, e.g. OS, username and etc. When you install any executable program in your PC, that program acquires necessary info to figure out where it is and where to navigate from that environment variable. To see yours, open command prompt and type "env". Among these variables, what is relevant to us is the highlighted one ``PATH``.

![](/posts/image01.png)

You can access ``PATH`` in R console too with ```Sys.getenv()['PATH']```

![](/posts/image02.png)



### <br>

### Rtools 설치 및 환경변수 추가하기 (step by step instruction)

#### <br>1. Rtools 설치하기 (Rtools Install)

아래 링크에 들어가서 시키는 대로 그대로 따라한다. 설치하면서 Next 만 누르고 다른건 건드리지 않는다. 이미 Rtools가 설치되어있는 경우 (C드라이브에 rtools40 폴더가 있다)에는 skip 

Click the link below follow the instruction exactly. Do NOT alter any setting in installation, e.g. path, and only click Next. If you do have ``C:\rtools40`` already installed, skip this step

https://cran.r-project.org/bin/windows/Rtools/

#### <br>2. ``PATH``에 추가하기 (Adding to PATH)

위에 링크에 방법을 소개하고 있긴 한데 그냥 제어판 들어가서 직접 설정하는 방법을 택하자. 윈도우에서 **제어판 > 시스템 > 고급 시스템 설정**을 클릭하면 다음과 같은 창이 뜬다. 여기서 환경변수 버튼을 눌러보자. 그리고 화면에 나온 대로 클릭하자.

The link above does have instruction on adding Rtools to PATH, but I chose a sure-fire method which is to use control panel yourself. (If unsure, always prefer to click click click to writing some code that can mess up good) Navigate to **Control Panel > System > Adv system setting**  and click environment variable button below and proceed as follows;

![](/posts/image03.png)



![](/posts/image04.png)



![](/posts/image05-1.png)

여기서 찾아보기를 눌러 경로 ``C:\rtools40\4.0\usr\bin``를 추가해줘야한다. **(왜냐면 이 경로에 ``make``파일이 있기 때문이다. 에러를 잘 보면 ``make``파일이 없다고 징징거릴거다. R콘솔이 경로를 못 찾겠다는 말이다.)** 그러면 화면처럼 맨 밑에 추가된 경로가 보인다. 

Now you should click the browse button and add the path ``C:\rtools40\4.0\usr\bin`` which has just been created after Rtools installation. **If you paid attention, you might have noticed that in the Rmarkdown knit error message there was something that went roughly as "cannot find file ``make``...".** **What the console told you is that it failed to find that very path we just added because it was NOT there before.** Once you added the path you would see the path loaded good in the last line as the picture above.

![](/posts/image07.png)

#### <br>3. 문제 해결 확인 (Restart Rstudio and knit again)

Rstudio를 다시 키고 나서 knit을 다시 돌리면 문제 해결! Boom!

![](/posts/image05.png)



#### <br>참고: Rendering Latex to PDF in Rmarkdown

가끔 Rmarkdown에서 Latex 수식을 썼을 때 knit가 되지 않을 때가 있을 것이다. 그때는 당황하지말고 침착히 ``tinytex::install_tinytex()``을 콘솔에 입력하면 된다. 시간이 걸린다. 설치 후 다시 시작하면 제대로 수식이 pdf로 나온다.

If you experience error rendering Rmarkdown file with Latex to pdf, just type in the console the command ``tinytex::install_tinytex()`` and restart Rstudio.



### <br>References

* https://stackoverflow.com/questions/39456577/cant-use-rcpp-engine-in-r-markdown/39470938#39470938 (solution to the same problem, a bit outdated)
* https://superuser.com/questions/284342/what-are-path-and-other-environment-variables-and-how-can-i-set-or-use-them (about path environment variable)
* https://thecoatlessprofessor.com/programming/cpp/installing-rtools-for-compiled-code-via-rcpp/ (if you have a problem installing Rcpp)
* https://bookdown.org/yihui/rmarkdown/language-engines.html#rcpp (how to use Rcpp in Rmarkdown)



### <br>Session info

![](/posts/image06.png)























