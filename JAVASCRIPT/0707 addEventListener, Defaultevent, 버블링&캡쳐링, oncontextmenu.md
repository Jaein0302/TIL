- **addEventListener()**
    - DOM객체에서 이벤트가 발생할 경우 **해당이벤트 처리 핸들러를 추가**할 수 있는 메서드
    - 이벤트가 발생하면 등록한 **이벤트 처리 핸들러가 실행**
    - **addEventListener(type, function_name, useCapture)**
        - **type**
            - 이벤트 타입을 나타내는 문자열로 'click', 'load', 'keydown' 등등
        - **function_name**
            - 리스너로 등록할 함수 이름
        - **useCapture**
            - **true →** 이벤트 흐름 중 캡쳐 단계에서 실행될 리스너를 등록
            **false →** 버블 단계에서 실행될 리스너를 등록. 생략 가능하면 디폴트는 false
    - **예시**
        - **changeflower.addEventListener('click', change);** →  **click** 이벤트가 발생할때 **change** 메서드를 호출하게 된다
        
        ```jsx
        <body>
        	<img id="image" src = "image/Penguins.jpg" width = "300" height = "330">
        	<script>
        		var changeflower = document.getElementById('image');
        		function change() {
        			changeflower.src = "image/Desert.jpg";
        			changeflower.width = 500;
        			changeflower.height = 500;
        		};
        	 changeflower.addEventListener('click', change);
        	</script>
        ```
        
- **Event 개체**
    - 자바스크립트 함수나 코드함수 호출시 인자값으로 event 또는 window.event를 보내게 되면 이벤트의 속성과 메서드를 알 수 있다
        - **type**
            - 현재 발생한 이벤트의 종류를 나타내는 문자열
        - **target**
            - 이벤트를 발생시킨 객체(태그)
        - **currentTarget**
            - 현재 이벤트를 실행하고 있는 DOM객체
        - **preventDefault()**
            - 이벤트의 디폴트 행동을 취소시키는 메서드
    - 예시
        
        ```jsx
        <script>
        	function myEvent(e) {
        			var message = "이벤트 종류는 " + e.type + "입니다. \n";
        			message += "이벤트를 발생시킨 객체(태그)는 " + e.target.tagName + "\n";
        			message += "현재 이벤트를 실행하고 있는 객체(태그)는 " + e.currentTarget.tagName + "\n";
        			alert(message);			
        		}
        </script>
        <body>
        	<input type = "button" value="버튼" onclick="myEvent(window.event)"> 
        </body>
        ```
        
- **숫자 더하기 기능 만들기**
    - 예시1 (선언적 함수 활용)
        
        ```jsx
        <script>
        		function add() {			
        			var a = document.getElementById('op1');
        			var b = document.getElementById('op2');
        			document.getElementById("result").value = parseInt(a.value) + parseInt(b.value);   // Number도 가능
        		}
        		
        	</script>
        	
        	<form>
        		<input id="op1" type="text" size="2"> +
        		<input id="op2" type="text" size="2"> 
        		<input type="button" value="=" onclick="add();">
        		<input id="result" type="text" size="2">	
        	</form>
        ```
        
    - 예시2 (addEventListener 활용)
        
        ```jsx
        <input id="op1" type="text" size="2"> +
        	<input id="op2" type="text" size="2"> 
        	<input id = "cal" type="button" value="=" >
        	<input id="result" type="text" size="2">	
        	<script>
        	// 이벤트 핸들러 - 특정 이벤트가 발생했을때 실행하고자 하는 자바스크립트 함수나 코드
        		function add() {			
        			var a = document.getElementById('op1').value;
        			var b = document.getElementById('op2').value;
        			var sum = document.getElementById('result');
        			sum.value = parseInt(a) + parseInt(b);
        			sum.style.backgroundColor = 'orange';
        		}
        		
        		var cal = document.getElementById('cal');
        		cal.addEventListener('click', add);
        	</script>
        ```
        
- **Defaultevent**
    - **일부 HTML 태그는 이미 이벤트 리스너**가 있는데 이것을 **디폴트 이벤트**라 한다
    - 예를들면, a 태그의 페이지 이동, submit버튼의 페이지 이동이 디폴트 이벤트이다
    - **DefaultEvent 제거방법1**
        - obj.**onsubmit**	= function(){
            **return false;**
        }
        
        ```jsx
        <script>
        		document.getElementById('my-form').onsubmit = function() {	
        				return false; // false를 리턴하면 입력양식을 제출하지 않는다
        		};
        </script>
        ```
        
    - **DefaultEvent 제거방법2**
        - **onsubmit의 값이 false**가 되도록 whenSubmit의 반환값을 설정해준다
        - function **whenSubmit()** {
            **return false;**
        }
        <form **onsubmit="return whenSubmit()"**>
        
        ```jsx
        <script>		
        		function whenSubmit() {
        			var name = document.getElementById('name');
        			var pw1 = document.getElementById('pass');
        			var pw2 = document.getElementById('pass-check');
        				
        			if (!name.value.trim()) {
        				alert('이름을 입력하세요');
        				name.focus();
        				return false;
        			} else	if (pw1.value.trim() == '') {
        				alert('비밀번호를 입력하세요');
        				pw1.focus();
        				return false;
        			} else if (pw2.value.trim() == '') {
        				alert('비밀번호확인을 입력하세요');
        				pw2.focus();
        				return false;
        			} 
        			
        			if (pw1.value == pw2.value) {
        				alert('성공')
        			} else {
        				alert('비밀번호가 다릅니다. 다시 입력해주세요');
        				pw1.value='';
        				pw2.value='';
        				pw1.focus();
        				return false;		
        			}			
        		}
        	</script>
        </head>  
        <body>
        	<form onsubmit="return whenSubmit()" action="ex30_test.html">
        		<fieldset>
        		<legend> 회원가입폼 </legend>
        			<label for = "name">이름</label>		
        			<input type="text" name="name" id="name"><br>
        			
        			<label for = "pass">비밀번호</label>		
        			<input type="password" id="pass"><br>
        			
        			<label for = "pass-check">비밀번호 확인</label>		
        			<input type="password" id="pass-check"><br>
        			
        			<input type="submit" value="제출">
        		</fieldset>
        	</form>
        </body>
        ```
        
    - **DefaultEvent 제거방법3**
        - event.preventDefault()를 사용한다
        - <a 	href="[http://www.naver.com](http://www.naver.com/)" onclick="**event.preventDefault()**">
        
        ```jsx
        <form onsubmit="return whenSubmit()" action="ex30_test.html">  //action이 없으면 자기 자신의 페이지로 돌아감(새로고침 느낌)
        		<fieldset>
        		<legend> 회원가입폼 </legend>
        			<label for = "name">이름</label>		
        			<input type="text" name="name" id="name"><br>
        			
        			<label for = "pass">비밀번호</label>		
        			<input type="password" id="pass"><br>
        			
        			<label for = "pass-check">비밀번호 확인</label>		
        			<input type="password" id="pass-check"><br>
        			
        			<input type="submit" value="제출" onclick="event.preventDefault()">
        		</fieldset>
        	</form>
        ```
        
    - **a 태그일때 DefaultEvent 제거방법**
        - **onclick = "event.preventDefault()"**
            - <a href = "[http://www.naver.com](http://www.naver.com/)" onclick="event.preventDefault()">
        - **onclick = "return false"**
            - <a href = "[http://www.naver.com](http://www.naver.com/)" onclick="return false">
        - **예시 (특정 단어 입력했을때 페이지로 안넘어가게 하기)**
            - 위 개념을 통해 onclick이 return false가 되어야 페이지가 안넘어가는 것을 알았다
            - 
        
        ```jsx
        <input id="hoho" type="text">
        	<br>
        	<a href="http://www.naver.com" onclick="return go();"> 입력 상자에 "하하"를
        		입력하면 네이버로 이동 안되요~</a>
        	
        	<script>
        		function go() {
        			var input = document.getElementById("hoho");		
        
        			if (input.value == '하하') {
        				alert('이동할 수 없습니다.')
        				return false;
        			}
        		};
        	</script>
        ```
        
- **버블링, 캡쳐링**
    - 버블링은 **자식 노드에서 부모 노드순**으로 **이벤트를 발생**하는 것을 의미하고 캡쳐링은 반대의 상황이다
    - **useCapture가 false**면 버블링 모드, **useCapture가 true**면 캡쳐링 모드를 쓴다는 것이다
    - 버블링, 캡쳐링의 해결방법은 **stopPropagation();** 메서드를 사용하는 것이다.
    - 다음 예시는 버블링이다
    
    ```jsx
    <body>
    	<div class = 'item' id= '외부div'>
    		<div class = 'item' id= '내부div'>
    			<h1 class='item' id='h1'>
    				<p class='item' id='Paragraph'>Paragraph</p>	
    			</h1>
    		</div>
    	</div>
    	
    	<script>
    	var items = document.querySelectorAll('.item');
    	//querySelector(선택자) - 선택자로 가장 처음 선택되는 문서 객체를 가져온다
    	
    	function something(e) {
    		var message = '현재 이벤트 실행하는 객체의 아이디 = '
    					+ e.currentTarget.id + "\n이벤트 발생 객체 태그 이름 =" + e.target.tagName;
    		alert(message);
    		
    		e.stopPropagation();  // 객체에 등록된 리스너를 실행 후 이벤트 흐름 중단
    	}
    	
    	for (var i=0; i<items.length; i++) {
    		items[i].addEventListener('click',something, false);		
    	}
    	</script>
    </body>
    ```
    
- **oncontextmenu**
    - 사용자가 브라우저의 바탕화면(body)이나 HTML 태그 위에 마우스 **오른쪽 버튼을 클릭**할 때 출력되는 메뉴를 context menu라고 한다
    - 오른쪽 클릭을 금지하기 위해서는 **oncontextmenu**가 **false를 반환**하면 된다
        
        ```jsx
        <script>
        	document.oncontextmenu = function() {
        		return false;
        	}
        </script>
        ```
        
- **radio 타입 Target을 이용하여 사진 바꾸기**
    - **window.event**란 웹 사이트의 코드가 현재 처리 중인 event를 반환하는 것을 의미한다
    - 따라서 click으로 발생한 **e**는 window.event와 같다
    - **currentTarget**은 이벤트를 등록해 놓은 요소를 반환하기 때문에 여기서는 **click한 icon[i]**의 값을 반환한다
    - **e.currentTarget에 있는 value값**을 **img의 src**에 넣으면 클릭했을때 이미지가 바뀌게 된다
        
        ```jsx
        <form>
        	<label><input type="radio" name="icon" value="image/Desert.jpg">image1</label>
        	<label><input type="radio" name="icon" value="image/flower.jpg" checked>image2</label>
        	<label><input type="radio" name="icon" value="image/koala.jpg">image3</label>
        	<img src='image/flower.jpg' id="iconImg">
        </form>
        <script>
        	var icon = document.getElementsByName("icon");
        	for (var i=0; i<icon.length; i++) {
        		icon[i].addEventListener('click', show);
        	}	
        	
        	function show(e) {		// e는 window.event와 같음
        		console.log(e === window.event)
        		var img = document.getElementById('iconImg');
        		img.src = e.currentTarget.value;		
        	}
        </script>
        ```
