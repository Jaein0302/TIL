# 0709 JQeury 기초, 선택자, add, remove, 충돌방지

- **JQUERY 사용방법**
    - **CDN방식**으로 가장 최신 버전을 불러온다
        - CDN(Content Delivery Network)은 사용자에게 간편하게 콘텐츠를 제공하는 방식으로
        네트워크에 데이터를 저장하여 제공하는시스템이다
        - **<script src = "[http://code.jquery.com/jquery-latest.js](http://code.jquery.com/jquery-latest.js)"></script>**
    - **직접 내려 받아 설치**한 경우 불러온다
        - 다운 한다음 파일지정 : **<script src="jquery-3.6.0.js></script>**
- **jQuery(document).ready()**
    - **jQuery(document)**
        - document를 jQuery 객체로 만든다
    - **ready(function)**
        - DOM이 완전히 로드되었을때 function을 실행한다
        - **window.onload**와 같은 기능
        - 문서가 준비되면 매개변수로 넣은 **콜백함수를 실행**하라는 의미 (변수 선언 순서와 상관없이 쓸 수 있음)
    - 예시
        - **jQuery(document).ready(function()**{
             console.log("hello jQuery");
        });
        - **$(function(){    →  단축형**
             console.log("간단한 형식");
        });
- **선택자**
    - **전체선택자**
        - $(function(){
             **$('*')**.css('color', 'yellow');
        });
    - **아이디선택자**
        - $(function(){
             **$('#a')**.css('color', 'orange');
        });
    - **클래스선택자**
        - $(function(){
             **$('.orange')**.css('color', 'orange');
        });
        - $(function(){
             **$('h1.item')**.css('background', 'red');   →  **h1 태그** 아래에 있는 **item 클래스** 지정
        });
        - $(function(){
             **$('.item.select')**.css('color', 'orange');   →  **item 클래스**와 **select 클래스** 두개 지정
        });
    - **태그선택자**
        - $(function(){
             **$('h1')**.css('color', 'orange');
        });
        - $(function(){
             **$('h1, p')**.css('color', 'orange');  →  **태그 h1, p 두개** 지정 가능
        });
    - **아이디선택자**
        - $(function(){
             **$('#target1’)**.css('color', result2);
        });
        - $(function(){
             **$('h1#target')**.css('color', 'orange');  →  **h1 태그** 아래에 있는 **target 아이디** 지정
        });
    - **조상, 자손, 형제**
        - **조상**
            - div ul
        - **자손**
            - div > ul
        - **형제**
            - div + ul
    - **속성선택자**
        - **$("input[type='text']")**
        - **$("input:text")**  →  단축형
    - **체이닝 기법**
        - 선택한 객체에 대해 메서드를 연속적으로 사용할 수 있는 기법
        - $("input:text")**.val**(name)
                              **.css**({'color':'green',
                                     'background':'orange',
                                     'font-size':'50pt'
                                      })
- **입력 양식 필터 선택자**
    - **checked (checkbox)**
        - 체크되어 있는 입력 양식을 선택한다
        - checked 된 데이터 불러오기
            - **$('input:checkbox:checked')**
                - 위의 문제점은 여러 checked된 데이터 중 **가장 위에 있는 것만 불러온다**는 점이다
                - 이것에 대한 해결책이 **each**이다
        - **each**
            - $(selector).**each(function(index,item){})**
                - **index →** 사용된 배열의 index
                - **item** → index에 해당하는 요소
                - 예시
                    
                    ```jsx
                    $(function() {
                    			setTimeout(function(){
                    			var list ="";  //  선언 위치 중요
                    			$('input:checkbox:checked').each(function(index, item){
                    					list += $(item).val() + " ";
                    					});
                    					alert(list)
                    				},3000)
                    			});
                    ```
                    
            - **$.each(object, fuction(index, item){})**
                - **object** → selector 불러온 객체
                - 예시
                    
                    ```jsx
                    $(function() {
                    			setTimeout(function(){
                    			var checkeds = $('input:checked');
                    			console.log(checkeds[0]);
                    			var list ="";  //  선언 위치 중요
                    			$.each(checkeds, function(){
                    					list += $(this).val() + " ";
                    					});
                    					alert(list)
                    				},3000)
                    			});
                    ```
                    
    - **checked (radio)**
        - checkbox와 형식이 같음
        - 예시
            
            ```jsx
            $(function() {
            			setTimeout(function(){
            			var imagepath = $('input:checked').val();
            			alert("선택한 그림의 위치는  '" + imagepath + "' 입니다");
            				},3000)
            			});
            ```
            
    - **attr**
        - **attr(속성명)**
            - 속성에 해당하는 속성값을 구해온다
        - **attr(속성명, 속성값)**
            - 속성에 해당하는 속성값을 설정한다
            - 예시
                - $('img').**attr('src',imagepath);**
                    - **img 태그**의 **src의 값**을 **imagepath**으로 설정한다
                - $('h1').**attr('class', 'high-light');**
                    - **h1태그**의 **class의 값**을 **high-light**으로 설정한다
        - **attr(속성명, function(index, attrValue){})**
    - **selected**
        - option 객체 중 선택된 태그를 선택한다
        - checked가 아니라 **select**으로 데이터를 부른다
        - 예시
            
            ```jsx
            $(function() {
            		setTimeout(function(){
            		var menu = $('select').val();
            		alert("선택한 메뉴는  '" + menu + "' 입니다");
            		},3000)
            	});
            ```
            
- **위치 필터 선택자**
    - 위치와 관련된 필터 선택자입니다
        - $(’li: **even’) →** 짝수 인덱스 요소 문서 객체를 선택한다
        - $(’li : **odd’) →** 홀수 인덱스 요소의 문서 객체를 선택한다
        - $('li:**eq(1)**')  → li 중 첫번째 요소
- **addClass()**
    - 태그에 class를 추가시킬때 사용
    - **예시**
    $(function() {
    	$('h1').**addClass('high-light');**
    });
    - **결과**
        - <h1 **class="high-light"**>item</h1>
- **removeClass()**
    - **예시**
        - $('h1:even').**removeClass**('high-light');
- **jQuery 충돌방지**
    - jQuery 외에 여러 플러그인([http://jQueryui.com/draggable/](http://jqueryui.com/draggable/))을 사용할때 $식별자가 충돌이 발생할 수 있다
    - 해결방법은 jQeury의 식별자로 **$를 사용하지 않고**, 다른 식별자로 지정하여 사용하는 것이다
    - 예시
        - **$.noConflict();**  →  더이상 jQeury의 식별자로 $를 사용할 수 없다
        **var q = jQuery;  →**  jQuery객체를 다른 변수에 저장해서 사용한다
            
            **q**(function() {
            	**q**('h1').addClass('append');
            });
