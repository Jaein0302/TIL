## 타이머 함수

- **setTimeout (function, millisecond)**
    - 일정 시간후 함수를 한 번 실행한다
    - 예시1) → 3초 후에 alert함수 실행
        
        ```jsx
        setTimeout(
        	 			function()
        	 			{alert('3초가 지났습니다');}
        				, 3000);
        ```
        
    - 예시2) → 3초 추에 다음주소로 이동
        
        ```jsx
        setTimeout(
        	 			function() {
        	 				location.href="http://naver.com"
        	 			}
        				, 3000);
        ```
        
- **clearTimeout(id)**
    - 일정 시간 후 함수를 한번 실행하는 것을 중지한다
- **setInterval(function, millisecond) :**
    - 일정 시간마다 함수를 반복해서 실행한다
    - 예1) 시계만들기
        - **setInterval (function, 1000)**의 형식
        
        ```jsx
        <script>        
        		setInterval(
        			function(){                       
        				var today = new Date();
        				document.body.innerHTML = today.toLocaleString();}   //document.write이랑 차이점 알아보자
        				,1000);    
        	</script>
        ```
        
        - **current라는 함수를 선언**하고, **setInterval(current, 1000)**의 형식
        
        ```jsx
        <script>        
        		function current(){                       
        			var today = new Date();
        			document.body.innerHTML = today.toLocaleString();
        		}
        		setInterval(current, 1000);
        	</script>
        ```
        
- **clearInterval(id)**
    - 일정 시간 후 함수를 반복하는 것을 중단한다
    - 여기서 id는 setTimeout()함수와 setInterval()함수를 사용하면 **타이머 아이디를 리턴**
    - 이 **타이머 아이디**를 **매개 변수**에 넣어주면 타이머를 **정지**할 수 있다

## Prototype

- 생성자 함수를 이용하여 속성을 저장할때, **공통된 속성**은 **prototype**으로 저장한다
- 함수 생성자를 통해 만들어진 객체는 prototype 속성을 통해 추가된 속성과 메서드를 **모두 공유**할 수 있다
- **예시**
    - 공통적으로 사용할 수 있는 **속성**과 **메서드**를 생성해보자
    - **Saram.prototype.ban = ‘2반’**
    - **Saram.prototype.commonMethod** = function(){
        console.log("공통 메서드입니다")
    }
    
    ```jsx
    <script>
    		function Saram(name_value, age_value, ki_value, blood_value, address_value) {
    			this.name = name_value;
    			this.age = age_value;
    			this.ki = ki_value;
    			this.blood = blood_value;
    			this.address = address_value;
    			
    			this.display = function() {
    				document.write("<table border='1'>");
    				document.write("<caption> 신상정보 </caption>");
    				document.write("<tr><th>항목 (속성: property)</th>");
    				document.write("<th>내용 (값:value)</th></tr>");
    				
    				for( var p in this) {
    					if (p != "display") {
    						document.write("<tr>");
    						document.write("<td>saram." + p + "</td>");
    						document.write("<td>" + this[p]);
    						if (p == "age")
    							document.write("세");
    						if (p == "ki")
    							document.write("cm");
    						if (p == "blood")
    							document.write("형");
    						
    						document.write("</td></tr>");
    					}
    				}
    				document.write("</table>");
    			}
    		}
    		Saram.prototype.ban = '2반';
    		
    		Saram.prototype.commonMethod = function(){
    			console.log("공통 메서드입니다")
    		}
    		
    		Saram.prototype.commonMethod();
    		
    		var na = new Saram("나최고", 25, 170.2, "A", "서울시 강남구");
    		na.display();
    		console.log("na.ban=" + na.ban)
    		na.commonMethod();
    		
    		var you = new Saram("너최고", 35, 180, "B", "서울시 종로구");
    		you.display();
    		console.log("na.ban=" + you.ban)
    		you.commonMethod();		
    	</script>
    ```
    
- 실행결과
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7e20b10b-0a3c-4ce4-b1a6-f088e077f89d/Untitled.png)
