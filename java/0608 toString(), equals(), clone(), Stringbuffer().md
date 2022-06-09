# 0608

- **과제 리뷰 - 달력만들기**
    - **월 첫째날 요일 구하기**
        - GregorianCalendar 클래스에 **생성자 초기화**
        - 1일의 요일이 필요한것이기 때문에 (년, 월, **1일**) 형식으로 초기화
            
            ```java
            Calendar c = new GregorianCalendar(year, month-1, 1);
            ```
            
        - **DAY_OF_WEEK**를 이용하여 1일에 해당하는 요일 구하기
            
            ```java
            int day_of_week = c.get(Calendar.DAY_OF_WEEK);
            ```
            
    - 마지막 날 구하기
        - **getActualMaximum 메서드** 이용
        - 해당 월의 실제 마지막 날을 구할 수 있음
            
            ```java
            int lastday = c.getActualMaximum(Calendar.DATE);
            ```
            
    - 숫자 출력하기
        - 1일 전까지는 공백으로 채운다
        - count는 7일이 채워지기 전까지 계속 증가시켜준다 (7이 되었을때 줄바꿈)
            
            ```java
            int count;
            	
            for(count = 1; count<day_of_week; count++) {
            		System.out.print("\t");
            }
            ```
            
        - 공백을 채웠으면 마지막날까지 숫자를 출력한다
        - 다만, 앞서 말한 count가 **7의배수**가 될때마다 줄바꿈을 해준다
            
            ```java
            for (int i=1; i <= lastday; i++) {
            		System.out.print(i + "\t");
            		
            		if (count++ % 7 ==0) {
            			System.out.println("\n");
            			}
            ```
            
- 로또 중복없이 나오게 하기
    - **i —**를 넣어주고 **break** 함으로써 i-1인 상태로 최근 반복문을 벗어나 가장 상단의 반복문으로 다시 들어가게 됨.
    - 거기서 다시 i-1이 **i**로 바뀌고 **r[i]**를 만들게 됨
    - 결론은 같은 숫자가 나올때마다 위로 올라가서 새로 생성
    - 모든 반복문이 끝났을때, r[i]를 출력
    
    ```java
    Random random = new Random();
    	
    	int[] r = new int [6];
    	for(int i=0; i<6; i++) {
    		r[i]=random.nextInt(45)+1;
    		
    		for(int j=0; j<i; j++) {
    			if (r[i]==r[j]) {
    				i--;
    				break;
    				}
    			}
    		}
    	for(int i=0; i<6; i++)
    		System.out.println(r[i]);
    	}
    ```
    
- **toString()**
    - **Object 클래스의 toString()**
        - **Object클래스**의 **toString**은 인스턴스에 대한 정보를 문자열로 제공할 목적으로 정의
        - 정의된 내용이 그대로 출력 : “ex13_1_Object_toString.Circle@606d8acf”
        
        ```java
        public class Circle {
        	int radius;
        	public Circle (int radius) {
        		this.radius = radius;
        	}
        }
        ```
        
        ```java
        		Circle obj1 = new Circle(5);
        		
        		System.out.println(obj1.toString());
        		System.out.println(obj1);
        ```
        
    - **오버라이딩한 toString()**
        - toString을 오버라이딩함
        
        ```java
        public class GoodsStock {
        	String goodsCode;
        	int stockNum;
        	
        	GoodsStock (String goodsCode, int stockNum) {
        		this.goodsCode = goodsCode;
        		this.stockNum = stockNum;
        	}
        	public String toString() {
        		return("상품코드:"+goodsCode + " 재고수량:" + stockNum);
        		
        	}
        ```
        
        - 오버라이딩함으로써 **obj.toString**이 **String**으로 변환됨
        
        ```java
        	GoodsStock obj = new GoodsStock("73527", 200);
        	String str = "재고 정보 = " + obj;
        	System.out.println(str);
        	}
        ```
        
- **equals()**
    - equals()를 오버라이딩하기
        - Circle클래스의 obj를 **업캐스팅**시켜서 불러들인다 (equals가 **Object타입**을 매개변수로 받기 때문)
        - obj가 **null이 아닌지**, Circle과는 **종속관계인지** 따진다(**다운 캐스팅**을 해줘야하기 때문)
        - 다운캐스팅하여 값 비교
        
        ```java
        public class Circle {
        	int radius;
        	Circle (int radius) {
        		this.radius = radius;
        	}
        	
        	public boolean equals(Object obj) {
        		if ((obj != null) && (obj instanceof Circle)) {
        			Circle c = (Circle)obj;
        			return (c.radius == this.radius);
        		} else {
        			return false;
        		}	
        	}
        }
        ```
        
        ```java
        		Circle2 obj1 = new Circle2(5);
        		Circle2 obj2 = new Circle2(5);
        		
        		if(obj1.equals(obj2))  
        			System.out.println("obj1.equals(obj2):같음");
        		else
        			System.out.println("obj1.equals(obj2):다름");
        ```
        
    - 각 클래스마다 equals()가 **Object**클래스 소속인지 **오버라이딩** 된건지 확인해봐야함
- **clone()**
    - GregorianCalendar 클래스 clone 하기
        - **Object imsi = obj1.clone();
        GregorianCalendar obj2 = (GregorianCalendar)imsi;**
        - 두문장은 하나로 줄일 수 있음 : **GregorianCalendar obj2 = obj1.clone();**
        
        ```java
        GregorianCalendar obj1 = new GregorianCalendar (2020,11,1);
        		
        		Object imsi = obj1.clone();
        		GregorianCalendar obj2 = (GregorianCalendar)imsi;
        		
        		System.out.printf("%tF %n", obj1);
        		System.out.printf("%tF %n", obj2);
        ```
        
- **ShallowCopy**
    - Point 클래스에 **toString 오버라이딩**해서 (x,y)로 출력할 수 있도록 함
    
    ```java
    public class Point {
    	int x, y;
    	
    	public Point (int x, int y) {
    		this.x = x;
    		this.y = y;
    	}
    	
    	public String toString() {
    		return ("(" + x + "," + y + ")");
    	}
    }
    ```
    
    - Circle 클래스에는 Cloneable 인터페이스 구현
        - **public class Circle implements Cloneable**
    - Clone() 메서드 오버라이딩
        - **(Circle) super.clone();** 에서 Circle로 형변환시켜서 return해준다 (더 간편함)
    - toString() 오버라이딩
    
    ```java
    public class Circle implements Cloneable {
    	Point p;
    	double r;
    		
    	Circle(Point p, double r) {
    		this.p = p;
    		this.r = r;
    	}
    	
    	public Circle clone() {
    		try {
    			return (Circle) super.clone();  
    		} catch(CloneNotSupportedException e) {
    			System.out.println("복제 오류");
    			return null;
    		}
    	}
    	
    	public String toString() {
    		return "[p=" + p + ", r=" + r + "]";
    	}
    }
    ```
    
    - 원래값을 변경하고 클론과 비교해봤을때 p의 값은 변하지 않는다 (**완벽한 Clone이 아니다)**
        - **Cloneable인터페이스를 구현한 클래스의 인스턴스만** 복제할 수 있기 때문 (Object클래스에 정의된 clone()은 인스턴스변수의 값만을 복제)
        - 인스턴스변수가 참조형일 때, 참조하는 객체도 복제되게 오버라이딩해야함.
    
    ```java
    Circle c1 = new Circle(new Point(1,1), 2.0);
    		Circle c2 = c1.clone();
    		
    		System.out.println("c1=" + c1);
    		System.out.println("c2=" + c2);
    		
    		c1.p.x = 9;
    		c1.p.y = 9;
    		c1.r = 4;
    		System.out.println("=====c1 변경 후=====");
    		System.out.println("c1=" + c1);
    		System.out.println("c2=" + c2);
    ```
    
- **DeepCopy**
    - ShallowCopy의 단점을 보완하기 위해 복제된 **Circle.Point의 참조변수가 새로운 Point 객체**를 만들 수 있도록 함
    
    ```java
    public Circle clone() {
    		try {
    			Circle c = (Circle) super.clone();  
    			c.p = new Point (this.p.x, this.p.y);  // c1 =this
    			return c;
    		} catch(CloneNotSupportedException e) {
    			System.out.println("복제 오류");
    			return null;
    		}
    ```
    
- **공변 반환 타입(covariant return type)-JDK1.5부터 적용**
    - 오버라이딩할 때 조상 메서드의 반환타입을 자손 클래스의 타입으로 변경을 허용하는 것
    - 즉, 반환형을 Object에서 자손 클래스 타입으로 변경을 허용한다
    - 좋은점 : clone()호출 후 **형 변환이 필요 없다.**(main 메서드)
    Rectangle obj2 = **(Rectangle)** obj1.clone ();
    ==> Rectangle obj2 = obj1.clone();
- **해싱**
    - 해시함수를 이용해서 해시테이블에 저장하고 검색하는 기법
    - 해시테이블이란 여러 개의 통(bucket)을 만들어 두고 키 값을 이용하여 데이터를 넣을 통 번호를 계산하는 자료구조
    - 결론은 어떤 대상을 찾기 쉽게 **해시코드값**을 저장하는 것
- **클래스 정보 알아내기**
    - 클래스 정보를 얻는 방법
        - Class c1 = p.**getClass();  → 생성된 객체**로부터 얻는 방법
        - Class c2 = **Point.class**; **→ 클래스 리터럴**로부터 얻는 방법
        - Class c3 = null; **→ 클래스 이름**으로부터 얻는 방법
        try {
        c3 = **Class.forName**("ex13_8_Object_getclass.Point");  
        } catch (ClassNotFoundException e) {
        e.printStackTrace();
        }
    - 클래스 정보를 출력하는 방법
        - System.out.println(c1.**toString**);  → toString은 생략 가능
        - System.out.println(c1**.getName()**); → 패키지명.클래스명
        - System.out.println(c1.**toGenericString()**); → 접근제어자 class패키지명. 클래스명
- **StringBuffer**
    - 쓰는 이유?
        - String은 변경이 불가능한데, StringBuffer는 가능함
    - **메소드테이닝**
        - **자기 자신의 메소드를 호출**하여 자기 자신의 값을 바꿔나가는 것 (성능이 String보다 좋음)
        - **String** str = **new StringBuffer().append("hello").append("").append("world")**.toString();
            - 계속해서 append해나가고 마지막 append한곳은 StringBuffer 자신이다.
        - 다음 5문장을 줄여쓴 것과 같다
            - StringBuffer sb = new StringBuffer();
            sb.append("hello");
            sb.append("");
            sb.append("world");
            String str = sb.toString();
