# 0627 CSS기초 & Pseudo

- **CSS**
    - 왜 쓰는지?
        - Cascading Style Sheet의 약자로서, HTML 문서에 색이나 모양, 출력위치 등 **웹 사이트의 외관을 꾸미는 언어**
- **Selector**
    - **body, h3, p** 와 같은 태그명들이 선택자의 역할을 한다
- **Property**
    - **background, color, font-size** 등이 속성값의 역할을 한다
    - **예시**
        - **<style>**
            **body** {**background** : **mistyrose**;}
            **h3** {**color** : **red**;}
            **p** {**font-size** : **10pt**;}
        **</style>**
- **단위**
    - **절대 단위**
        - cm : 센티미터
        - mm : 센티미터 1/10
        - in : 1인치는 2.54cm
        - **pt** : 1포인트는 1/72인치
        - pc : 1파이커는 12포인트
    - **상대 단위**
        - **px** : 1픽셀을 1로 하는 단위로 디스플레이의 해상도에 따라 크기가 달라진다
        - % : 다른 기준이 되는 크기에 대한 비율로 지정
        - **em** : 기본 글꼴 크기의 상대적 크기로 기본 글꼴 크기의 두배로 나타내고자 한다면 2em이라고 기술한다 IE경우 1em은 12pt, 16pt와 같다
- **cacading**
    - **부모** 요소로부터 **자식** 요소로 단계별로 스타일이 적용된다고 해서 **cascading(누적)**의 개념을 가지고 있다
    - 자식 태그는 부모의 style을 상속받는다
- **스타일 시트 적용방법**
    - **내부 스타일시트**
        - head 내부에 스타일 시트 작성
        - <html>
              **<head>**
                    <meta charset="EUC-KR">
                    **<style>**
                          body {background : mistyrose;}
                          h3 {color:red;}
                          *p {font-size:10pt;}*  
                     **</style>
              </head>
              <body>**
                    <h3>스타일시트 이해하기</h3>
                    <p> 이 예제는 <strong>CSS</strong>의 개념을 설명한다
              **</body>**	
        </html>
    - **외부 스타일시트**
        - 외부에 스타일시트를 만들어서 호출함
        - **rel** 속성 : 링크된 문서와의 관계을 지정함
        **type** 속성 : 속성값으로 text/css를 지정.(text유형의 css파일)
        **href** 속성 : 링크할 파일의 위치 정보를 지정
        - <html>
              **<head>**
                    <meta charset="EUC-KR">
                    **<link rel="stylesheet" type="text/css" href="ex04_extern.css">
              </head>
              <body>**
                    <h3>스타일시트 이해하기</h3>
                    <p> 이 예제는 <strong>CSS</strong>의 개념을 설명한다
              **</body>**	
        </html>
    - **인라인 스타일시트**
        - 태그안에 스타일시트를 적용
        - <strong **style="color:green; font-style:italic"**>CSS</strong>
- **전체선택자**
    - * → 모든 태그를 선택
    - <style>
          ***** {color : red}
    </style>
- **Class, ID**
    - **Class**
        - <strong **class**="red1">CSS</strong>
        - <style>
            **strong.red1** {font-size: 22pt}
        </style>
        - strong 태그 중에서도 red1 클래스 선택자를 의미한다
        - 클래스는 ****. ****을 붙인다
    - **ID**
        - <p **id**="next">CSS</p>
        - <style>
            **#next** { color: blue; text-align: center}
        </style>
        - ID 선택자는 앞에 **#** 을 붙인다
- **Pseudo class**
    - **가상클래스** 선택자라고 하며, 웹문서의 소스에는 실제로 존재하지 않지만 필요에 의해 임의로 **가상의 선택자를 지정**하여 사용하는 것
    - **:active**
        - 사용자에 의해 활성화된 요소를 선택(마우스를 **클릭**한 태그 선택)
        - h1:**active**{color:blue}
    - **:hover**
        - 마우스 커서가 위치한 요소를 선택(마우스를 **올린** 태그)
        - h1:**hover**{color:red}
    - **:focus**
        - 초점이 맞추어진 **input태그를 선택**
        - input:**focus**{background-color:orange;}
    - **:link**
        - 하이퍼링크 요소에 대한 링크를 의미(**방문하지 않은 링크** 스타일)
    - **:visited**
        - **방문한 링크**의 경우를 의미
    - **::after**
        - a {**text-decoration: none;**}  →  링크 밑줄이 사라짐
        a:link**::after**{**content**: ' - ' **attr**(href);}  →  a 태그 다음 링크 다음에 나오는 내용을 추가해서 출력해라 (**content** : 다음 / **attr** :  href의 속성값)
        <h1><a href="[http://www.daum.net](http://www.daum.net/)">다음</a></h1>
        - 결과값
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5e6ecf21-75ad-4dba-95ba-134b313d9259/Untitled.png)
        
    - **::before**
        - h3**::before** { content: "◆"; color: blue}
    - **::first-letter**
        - 문자 블록의 **첫번째 글자**를 선택
        - p**::first-letter** {font-size: 12pt;}
    - **::first-line**
        - 문자 블록의 첫번쨰 줄을 선택
    - **::selection**
        - 드래그 하는 부분에 적용
        - p**::selection**{color:yellow; background:black}
