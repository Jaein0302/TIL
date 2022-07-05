# 0630 자바스크립트 기초, Variable, innerHTML

- **Document**
    - Document 객체는 웹브라우저와 관련된 객체
    - **document.write()**
        - write method를 이용해서 **HTML 문서내에 콘텐츠를 삽입**
        - 화면에 그대로 표시되는 것이 아니라 **HTML문서에 추가**되는 것
    - **Method VS Function**
        - **Method**
            - 객체 안의 함수
        - **Function**
            - 특정 작업을 수행하기 위한 독힙적으로 만들어진 하나의 단위
            - return값이 존재한다
            - 객체에 독립적
- **자바스크립트 코드의 위치**
    - **HTML 태그의 이벤트 리스너 속성에 작성**
        - <img src="koala.jpg" **onclick="this.src='Desert.png'"**>
    - **<script></script>내에 작성**
        - <script>
             **alert("자바스크립트 안녕");**
        </script>
    - **자바스크립트 파일에 작성**
        - **<script src="welcome.js"></script>**
    - **URL 부분에 작성**
        - **<a href**="javascript:alert('Hello World')">환영합니다.**</a>**
- **Variable**
    - 자바스크립트 변수는 값이 저장되는 순간 자료형이 결정된다
    - input   →   **변수**
    Array.length    →   **속성**(length)  (자바로 치면 필드)
    alert('Hello world')   →   **함수**
    Math.abs(-273)   →  **메서드**(abs)
    - 종류
        - **string**
            - **큰 따옴표** 또는 **작은 따옴표**를 사용해서 문자열을 만든다
            - var name = "홍길동”
        - **number**
            - var start = 0
        - **boolean**
            - var booleanVar = true
        - **function**
            - var functionVar = function() {
                alert ('오늘도 수고하셨습니다');
            };
        - **object**
            - var objectVar = {};
        - **undefined**
            - **선언되지 않거나 할당되지 않은 변수**를 나타내는 변수형 (초기값 X)
            - var noVar;
- **형변환**
    - **묵시적(암시적) 형변환**
        - **숫자형** + 문자형 →  숫자형이 **문자형**으로 변환
            - “3” + 5 + 2  →352
        - **불린형** + 문자형 →  불린형이 **문자형**으로 변환
            - var a = true;
            a + “1” → true1
        - **불린형** + 숫자형 → 불린형이 **숫자형**으로 변환(true=>1, false=>0)
            - var a = true;
            a + 1 → 2
    - **비교연산자**일때 형변환
        - **문자열** < 숫자형  →  문자열이 **숫자형**으로 변환 
        1 < "2”  →  true
        - 만약 문자열이 숫자형이 아닐경우 **NaN(Not a Number)**
        "A" > 1  →  false
        - 일치 연산자 (===)  :  자료형과 값이 모두 일치해야함
            - '1 === "1”   →  false
            - "1" === "1”  →  true
    - **boolean 비교**
        - **alert**
            - **alert(1==true) →** true가 숫자 1로 변환된 후 비교
            - **alert(0==false) →** false가 숫자 0로 변환된 후 비교
        - **if**
            - if(0) → false (숫자 0은 false)
            - if(1) → true
            - if(’’) → false(빈문자열은 false)
            - if(’호호’) → true
            - if(null) → false(null은 false)
        - **NaN**
            - 숫자가 아닌경우에 true
- **innerHTML**
    - **document.body.innerHTML**
        - body 내부에 해당하는 문장을 HTML로 출력
        - 만약 script가 body가 아닌 head에 있을 경우 **body가 빈 객체**이기 때문에 값을 저장하면 에러가 생긴다
            - **<head>**
                <meta charset="UTF-8">
                <title>innerHTML 속성</title>
                <script>
                    **document.body.innerHTML = '안녕하세요?';**
                </script>
            **</head>**
            <body>
            </body>
        - **특징 (주의점)**
            - document.body.innerHTML을 실행할때 **<script> 이전 문장들은 실행되지 않는다**
            - 왜냐하면 document.body.innerHTML은 script에서도 가장 마지막인 코드만 출력한다 (list에 +를 쓰는 이유)
                
                ```java
                body>
                		<p>
                			저는 안보여요
                			Title1 : 간단한 화면 출력
                		</p>
                		<script>
                			var title1 = "대한민국 전국 맛집";
                			var title2 = "지적 대화를 위한 넓고 얕은 지식";
                			var title3 = "베스트셀러 절대로 읽지 말라";
                
                			var list = '';
                			
                			list += "<table>";
                			list += "<caption> 베스트셀러 </caption>";
                			list += "<tbody>";
                			list += "<tr>";
                			list += "<th> 순위 </th>";
                			list += "<th> 제목 </th>";
                			list += "</tr>";
                			list += "<tr> <td> 1 </td> <td> " + title1 + " </td> </tr>";
                			list += "<tr> <td> 2 </td> <td> " + title2 + " </td> </tr>";
                			list += "<tr> <td> 3 </td> <td> " + title3 + " </td> </tr>";
                			list += "</tbody>"
                			list += "</table>"
                			
                			document.body.innerHTML = list;
                		</script>
                		<p>Title2 : 간단한 화면 출력</p>
                	</body>
                ```
                
        - 해결방법
            - <script> 전의 문장까지 나오게 하려면 **document.body.innerHTML += list**;라고 바꿔야한다
            - document.body.innerHTML = **document.body.innerHTML** + list 에서 두번째 **document.body.innerHTML**은 list없이 출력되는 것이기 때문에 <script>전의 문장들이 출력된다.
            
            ```java
            <body>
            		<p>
            			저는 보여요
            			Title1 : 간단한 화면 출력
            		</p>
            		<script>
            			var title1 = "대한민국 전국 맛집";
            			var title2 = "지적 대화를 위한 넓고 얕은 지식";
            			var title3 = "베스트셀러 절대로 읽지 말라";
            
            			var list = '';
            			
            			list += "<table>";
            			list += "<caption> 베스트셀러 </caption>";
            			list += "<tbody>";
            			list += "<tr>";
            			list += "<th> 순위 </th>";
            			list += "<th> 제목 </th>";
            			list += "</tr>";
            			list += "<tr> <td> 1 </td> <td> " + title1 + " </td> </tr>";
            			list += "<tr> <td> 2 </td> <td> " + title2 + " </td> </tr>";
            			list += "<tr> <td> 3 </td> <td> " + title3 + " </td> </tr>";
            			list += "</tbody>";
            			list += "</table>"
            			
            			document.body.innerHTML += list;
            		</script>
            		<p>Title2 : 간단한 화면 출력</p>
            	</body>
            ```
            
- **alert**
    - 확인 버튼이 있는 대화상자
    - <script>
        **alert(**"HTML5 프로그래밍 \n웹사이트로 이동한다")    →  script내부이기 때문에 **\n** 대신에 <br>을 사용하면 안된다
    </script>
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3ec18e61-197f-4ed7-88d4-a2d912b936f0/Untitled.png)
        
- **confirm**
    - **확인**, **취소** 버튼이 있는 대화 상자
    - <script>
    var answer = confirm("장바구니로 이동할까요?");
    var list = "Answer = " + answer + "<br>";
    
    if (answer) {
    	list += "<img src= 'image/Desert.jpg' width=300px>";
    } else {
    	**list += "<style>body{background: yellow}</style>";  →  1번째 방법
            document.head.innerHTML = "<style>body{background: yellow}</style>";   →  2번째 방법 (head에 적용)
            document.body.style.background = "yellow";  →  3번째 방법**
    }
    	**document.body.innerHTML = list;**
    </script>
    - 확인 → **true**
    - 취소 → **false**
