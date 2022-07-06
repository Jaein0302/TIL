# 0701 prompt, date, math

- **prompt**
    - 형식 : **result = prompt(message, value)**
    - 문자열만 입력받을 수 있다
    - **예시**
        - var answer = **prompt("주소를 입력하세요", "서울시 종로구");**
        **var output** = "Answer = " + answer + "<br>";
        **document.body.innerHTML = output;**
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/31cf6905-c4d8-4709-bdee-fe61ad210b49/Untitled.png)
            
    - **prompt로 구구단 만들기**
        - **취소**를 누르면 **null**로 적용된다
        
        ```java
        <script>
        var dan = prompt("단을 입력하세요");
        			var output = "";
        			
        			if(dan == null) {
        				alert("취소를 누르셨군요")
        			} else {
        				if(dan.trim()) { // 데이터를 입력한 경우, 공백 제거하는 메서드 trim() 사용
        					if(isNaN(dan)) {  // 숫자가 아닌경우
        						alert("숫자만 입력 가능합니다")
        					} else {
        						output = "입력 단 = " + dan + "<br>"
        						for (var i = 1; i <= 9; i++) 
        							output +=  dan + '*' + i + '=' + i*Number(dan) + '<br>';
        					
        						document.body.innerHTML = output;
        					}
        				} else {  // 데이터를 입력하지 않은 경우
        					alert('데이터를 입력하세요');
        				}
        			}
        </script>
        ```
        
    - **prompt로 책 주문 로직 만들기 (if else)**
        - 테이블 만드는 코드 중간에 필요한 조건식을 넣어야 함
        
        ```java
        <script>
        		var book1 = "자바스크립트";
        		var book2 = "HTML";
        		var book3 = "CSS";
        		var title;
        		var n = prompt("주문할 책 수량을 입력 하세요?[1이상]", "1");
        		var count = Number(n);
        		
        		if(count >= 1){
        			var list = "<table>";
        			list += "<caption> 책 주문 입력 내용 </caption>";
        			list += "<thead><tr>";
        			list += "<th> 순번 </th>";
        			list += "<th> 제목 </th>";
        			list += "</tr></thead><tbody>";
        			
        			var book_list = "1." + book1 + "\n" + "2." + book2 + "\n" + "3." + book3;
        			for (var i=1; i<=count; i++) {
        				var choice = prompt("책 제목을 번호로 선택하세요... \n" + book_list, "1");
        				if (choice == "1")   // ""없어도 됨
        					title = book1;
        				else if (choice == "2")
        					title = book2;
        				else if (choice == "3")
        					title = book3;
        				else {
        					alert('리스트에 없는 책을 선택하셨습니다');
        					i--;
        					continue;
        				}
        				
        				list += "<tr>";
        				list += "<td>" + i + "</td>";
        				list += "<td>" + title + "</td>";
        				list += "</tr>";
        			}
        			
        			list += "</tbody></table>";
        			
        			document.body.innerHTML = list;
        		} else if (n==null) {
        			alert('취소를 선택하셨습니다');
        		}	else {		
        			alert('형식에 맞게 입력하세요=숫자로 1이상을 입력합니다.')
        		}
        		
        		</script>
        ```
        
    - **prompt로 책 주문 로직 만들기 (switch)**
        
        ```java
        <script>
        		var book1 = "자바스크립트";
        		var book2 = "HTML";
        		var book3 = "CSS";
        		var title;
        		var n = prompt("주문할 책 수량을 입력 하세요?[1이상]", "1");
        		var count = Number(n);
        		
        		if(count >= 1){
        			var list = "<table>";
        			list += "<caption> 책 주문 입력 내용 </caption>";
        			list += "<thead><tr>";
        			list += "<th> 순번 </th>";
        			list += "<th> 제목 </th>";
        			list += "</tr></thead><tbody>";
        			
        			var book_list = "1." + book1 + "\n" + "2." + book2 + "\n" + "3." + book3;
        			for (var i=1; i<=count; i++) {
        				var choice = prompt("책 제목을 번호로 선택하세요... \n" + book_list, "1");
        				//choice와 대응하는 case를 판별할때는 일치 연산자 (===)가 사용된다.
        				//따라서, 아무런 타입변환 없이 표현식 값이 같아야한다.
        				switch (choice) { //choice의 자료형은 string
        				case "1":
        					title = book1;
        					break;
        				case "2":
        					title = book2;
        					break;
        				case "3":
        					title = book3;
        					break;
        				default:
        					alert('리스트에 없는 책을 선택하셨습니다');
        					i--;
        					continue;
        				}
        				
        				list += "<tr>";
        				list += "<td>" + i + "</td>";
        				list += "<td>" + title + "</td>";
        				list += "</tr>";
        			}
        			
        			list += "</tbody></table>";
        			
        			document.body.innerHTML = list;
        		} else if (n==null) {
        			alert('취소를 선택하셨습니다');
        		}	else {		
        			alert('형식에 맞게 입력하세요=숫자로 1이상을 입력합니다.')
        		}
        		
        		</script>
        ```
        
- **date**
    - 메서드 종류
        - getFullYear() - 연도 (yyyy)
        - getMonth() - 월(0:1월 ~ 11:12월)
        - getDate() - 일
        - getDay() - 요일(0:일요일 ~ 6:토요일)
        - getHours() - 시간(0~23)
        - getMinutes() - 분
        - getSeconds() - 초
        - getMilliseconds() - 밀리초(1/1000초)를 리턴(0~999)
        - getTime() - 1970년 1월 1일 부터 경과된 시간을 밀리초로 구함
        - toLocaleString() - 객체의 시간 정보를 local 표현의 문자열로 리턴
- **math**
    - Math.ceil(x) : x보다 크거나 같은 가장 작은 정수를 리턴한다.
    - Math.floor(x) : x보다 작거나 같은 가장 큰 정수를 리턴한다.
    - Math.roundl(x) : x를 반올림하여 리턴한다.
