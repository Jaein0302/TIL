# 0703 Object, Function

## Object

- **객체 생성법**
    - **new 연산자** 이용하기
        - var saram = new Object();
    - **객체 리터럴 {}** 이용하기 (키와 값의 쌍으로 작성)
        - var saram = {
                        'name'		: "나최고",
                        "age"		: 25	,
                         ki			: 170.2	,
                         blood		: 'A'	,
                         address		: '서울시 강남구'
        };
- **객체 출력방법**
    - **객체.속성이름**
        - document.write(saram.name);
    - **with 사용**
        - **with(saram)**{document.write(name);
    - **객체["속성이름"]**
        - document.write(**saram["name"]**);
    - **for문**
        - **for(var p in saram)** {
              document.write(p + ":" + saram[p] + "<br>")
        }
- **속성 삭제**
    - **delete** saram.address;
- **innerHTML을 이용하여 속성들 출력하기**
    - document.body.innerHTML은 script **내부에 작성한 문장(list)**을 body에 HTML화시켜서 출력하는 것이다
    - **if(p != 'print')**의 의미는 p의 속성중 출력하는 메서드인 **print만 제외**하고 모두 list에 문장들을 누적시켜서 나중에 출력할때 쓰인다는 의미이다.
    
    ```java
    <script>
    			var saram = new Object();
    			
    			saram.name = "나최고";
    			saram.age = 25;
    			saram.ki = 170.2;
    			saram.blood = "A";
    			saram.address = "서울시 강남구";
    
    			saram.print=function(){
    				var list="";
    				for(var p in saram) {
    					if(p != 'print')
    						list += p + '=' + saram[p] + "<br>";
    				}
    				list += "<hr>";
    				document.body.innerHTML = list;
    			};
    			
    			saram.print();		
    	</script>
    ```
    
- **계층이 있는 Object를 출력하기**
    - 만약 아래와 같이 객체 안에 객체**(info)**가 있다고 가정해보자
        
        ```java
        var book = {
        					title : "수미네 반찬",
        					publisher : "성안당",
        					author : "김수미",
        					price : 15300,
        					info : {
        							pages : 442,
        							date : "2010년 1월 30일",
        							ISBN10 : "8970506470",
        							size : "188*243mm"
        						}
        					};
        ```
        
    - 위 속성들을 이용하여 표를 출력해 보겠다
        - **for (var p in this)**  →  this는 book을 의미한다
        - **if (typeof (this[p]) != "object")**  →  book의 속성 중 object를 걸러내기 위함이다
            
            (info는 book의 속성이기도 하지만, 또다른 객체(object)이기 때문이다
            
        
        ```java
        book.print = function()	{
        			var list = "<table>";
        			list += "<caption> 서적 정보 </caption>";
        			list += "<tr>";
        			list += "<th> 항목(속성:property) </th>";
        			list += "<th> 내용(값:value) </th>";
        			list += "</tr>";
        			
        			for (var p in this) {
        				if (p != 'print') {
        					if (typeof (this[p]) != "object") {
        						list += "<tr>";
        						list += " <td> book." + p + "</td>";
        						list += " <td> " + this[p];
        						if (p == "price")
        							list += "원";
        						list += " </td>";
        						list += " </tr>";
        					} else {
        						for ( var q in this[p]) {
        							list += "<tr>";
        							list += "<td> book." + p + "." + q + "</td>";
        							list += "<td> " + this[p][q];
        							if (q == "pages")
        								list += "페이지";
        							list += "</td>";
        							list += "</tr>";
        						}
        					}
        				}
        			}		
        		list += "</table>";
        		
        		document.body.innerHTML = list;
        		
        		}
        		
        		book.print();
        ```
        
    - 실행결과
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6155c5d8-6fc7-45c7-b245-d7471e3416ac/Untitled.png)
        

## Function

- **함수 종류**
    - **함수 표현식**
        - 익명함수라고도 함.
        - 함수리터럴로 **특정변수에 할당**되거나 **즉시 실행가능한 코드 블럭**으로서 존재하는 함수를 의미
        - **특정변수에 할당(functionVar)**
            - var functionVar = **function () {**
                 alert("출력");	
            }
            - **functionVar**  →  함수호출
        - **즉시 실행가능한 코드 블럭**
            - **(function () {
                alert("즉시 실행합니다");
            })();**  →  즉시 실행
        - **functionVar is not a function error**
            - 함수 호출하는 부분인 functionVar();가 함수 표현 앞에 나오면 호출할 함수가 없기 때문에 에러가 생긴다
    - **함수 선언식**
        - 이름이 있는 함수
        - 위치에 상관없다 ( **함수 호출 부분이 함수 생선 전**에 사용해도 된다)
            - **function first() {**
                alert("즉시 실행합니다");
            }
            - **first();**  →  **함수 호출 (위치 상관없음)**
- **sumAll**
    - 가변인자함수 → 함수를 선언할 때와 다르게 호출시 **인자의 개수를 다르게 사용**하는 함수
        - **선언**
            - function sumAll() {
                document.write(typeof (arguments) + ' : ' + arguments.length + '<br>');
                document.write(arguments[0]	+ '<br>');
                document.write(arguments[1]	+ '<br>');
                document.write(arguments[8]	+ '<hr>');			
            }
        - **호출**
            - sumAll(1,2,3,4,5,6,7,8,9);
- **return**
    - **함수를 호출한 곳으로 돌아가라는 의미**다
    - return 키워드를 사용하면 **값을 지정하지 않아도 함수를 호출한 곳으로 돌아간다**
        - output에는 돌려받은 값이 없기 때문에 undefined가 뜬다
        
        ```java
        <script>
        		//함수 선언
        		function returnFunction() {
        			alert('문장A');
        			return;
        		}
        		var output = returnFunction(); 		
        		alert(typeof(output) + ' : ' + output);
        	</script>
        ```
        
- **return된 함수를 호출하는 방법**
    - **함수 선언**
        
        ```java
        function returnFunction() {
        			return function () {
        				alert('Hello Function')	;
        			};
        		}
        ```
        
    - **호출하는 방법**
        - return된 문장을 **document.write**에 넣는다
            
            ```java
            var result = returnFunction();
            		document.write(result);  //  function () { alert('Hello Function') ; }
            		result();
            ```
            
        - 호출하는 함수에 **()를 한번 더 사용**해야 한다
            
            ```java
            returnFunction()();
            ```
            
        - **즉시 실행가능한 함수**를 실행한다 (마찬가지로 **()를 한번 더 사용**)
            
            ```java
            (function () {
            			return function () {
            				alert('Hello Function 즉시');
            			};
            		})()();
            ```
            
- **함수안의 함수**
    - **callTenTimes 선언**
        - **callTenTimes**라는 함수에 **callback**이라는 함수를 매개변수를 사용한다고 가정하자
        
        ```java
        function callTenTimes(callback) {
        			for (var i = 0; i < 10; i++) {
        			callback(i);
        			}
        		}
        ```
        
    - **callback 선언**
        
        ```java
        callback = function(index) {
        	  				alert("함수 호출 : " + index);
        }
        ```
        
    - **callTenTimes 호출 (callback 선언부분을 같이 합쳐서 호출한다)**
        
        ```java
        callTenTimes(function (index) {
        						alert("함수 호출 : " + index);
        		}	
        );
        ```
        
- **지역변수 VS 전역변수**
    - 함수안에 var로 사용된 변수 →  **지역변수 (함수 외부에서 사용 못함)**
        - **function** test(name) {
            **var output** = 'Hello' + name + ' .. !';
        document.write(output);
        }
    - **함수 밖**에서 선언된 변수 OR 함수안에서 **var 생략**한 변수 → **전역 변수**
        - **function** test(name) {
             **output** = 'Hello' + name + ' .. !';
        document.write(output);
        }
    - **클로저**
        - 아래와 같이 함수안에 선언된 **output**이라는 **지역변수**가 있다고 가정하자
        
        ```java
        function test(name) {
        			var output = 'Hello' + name + ' .. !';
        			return function() {
        				alert(output);
        			};
        		}
        ```
        
        - output은 당연히 지역변수이기 때문에 호출할때 사용할 수 없다
        - 하지만, return문에 있는 output은 이미 함수에 의해 반환된 값이기 때문에 쓸 수 있다
            - **test('자바스크림트')();  → 에러 없음**
        - 만약 alert의 매개변수로 output을 사용하려면 에러가 생긴다
            - **alert("outside = " + output);  → 에러 발생(output is nnot defined)**
