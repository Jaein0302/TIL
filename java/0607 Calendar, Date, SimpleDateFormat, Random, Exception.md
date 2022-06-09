- **과제리뷰 (break와 continue를 적절한 곳에 쓰기)**
    - 주민등록번호를 차례대로 숫자가 아닌 경우를 비교한다. 다만, **‘-’인 경우는 비교하지 않는다**
        - 나의 방법
            - **replace**를 이용하여 ‘-’를 빈 문자열로 바꿨다
                
                ```java
                int isNumber (String zumin) {
                		zumin = zumin.replace("-","");
                		for (int i=0; i<zumin.length(); i++) {
                			char ch = zumin.charAt(i);
                			
                			if((ch<'0' || ch>'9') && ch!='-')
                				result = i;
                ```
                
        - 답안
            - 해당 자리에 있을때 **continue**를 통해 비교하는 절을 건너뛰었다
            - index = -1로 지정함으로써 주민번호가 숫자라고 만족되는 경우를 초기값 -1로 만들었다.
            - **break**는 반복문을 벗어나는 문장이기 때문에 바로 return index로 갈 수 있다
                
                ```java
                static int isNumber(String zumin) {
                		int index = -1;
                		for (int i=0; i<zumin.length(); i++) {
                			
                			if(i==6)
                				continue;
                			
                			char test = zumin.charAt(i);
                			if (test < '0' || test > '9') {
                				index = i;
                				break;
                			}
                		}
                		return index;
                	}
                ```
                
- **Calendar 클래스**
    - **Calendar today = Calendar.getInstance();**
        - Calendar는 추상클래스이기 때문에 **메서드**를 통해 **인스턴스를 얻어오는 방법**
        
        ```java
        Calendar today = Calendar.getInstance();
        		
        		System.out.println("이 해의 년도:"+today.get(Calendar.YEAR));
        		System.out.println("월(0~11, 0:1월):" + today.get(Calendar.MONTH));
        ```
        
    - **Calendar cal = new GregorianCalendar();**
        - **GregorianCalendar**를 Calendar으로부터 **상속 받아 구현한것 = GregorianCalendar cal = new GregorianCalendar  이거와 같음**
        - Today 생성자로 **GregorianCalendar 형식**을 매개변수로 하여 초기화시킴
        - Today라는 클래스에 **toString을 오버라이딩**하여 언제든지 출력할 수 있도록 만듬
        
        ```java
        public class Today {
        	private Calendar cal = new GregorianCalendar();
        	
        	public Today() {}
        	
        	public Today(GregorianCalendar cal) {
        		this.cal = cal;
        	}
        	
        	public String toString() {
        		final String [] DAY_OF_WEEK = {"", "일", "월", "화", "수", "목", "금", "토"};
        		final String [] AM_PM = {"오전", "오후"};
        		
        		return(cal.get(Calendar.YEAR) + "년 " + (cal.get(Calendar.MONTH)+1) + "월 " 
        		+ cal.get(Calendar.DATE) + "일 " + AM_PM[cal.get(Calendar.AM_PM)] + " " + cal.get(Calendar.HOUR) 
        		+ ":" + cal.get(Calendar.MINUTE) + ":" + cal.get(Calendar.SECOND) + "초 " 
        		+ DAY_OF_WEEK[cal.get(Calendar.DAY_OF_WEEK)] + "요일");
        	}
        }
        ```
        
        - **Today 객체**를 생성시켜 출력하면 현재 시간이 출력됨 (Today의 toString 작동)
        
        ```java
        public class Today_test {
        	public static void main(String[] args) {
        		System.out.println("=========오늘의 날짜===========");
        		Today t = new Today();
        		System.out.println(t.toString());
        		System.out.println(t);
        	}
        }
        ```
        
    - **GregorianCalendar g = new GregorianCalendar();**
        - **GregorianCalendar 객체**를 만들어서 사용가능
        - **print 메서드** 호출
        
        ```java
        GregorianCalendar c = new GregorianCalendar();
        		
        		c.add(Calendar.DATE, 1);
        		System.out.println("1 증가된 날 =" + c.get(Calendar.DATE));
        		print(c);
        ```
        
        - print 메서드 내에서 **Today 객체 생성 및 생성자 초기화**
        - **toString 메서드 호출**
        
        ```java
        private static void print(GregorianCalendar c) {
        		Today t = new Today(c);
        		System.out.println(t.toString() + "\n");
        	}
        ```
        
        - **toString**을 이용해 원하는 형식 출력
        
        ```java
        public class Today {
        	private Calendar cal = new GregorianCalendar();
        	
        	public Today() {}
        	
        	public Today(GregorianCalendar cal) {
        		this.cal = cal;
        	}
        	
        	public String toString() {
        		final String [] DAY_OF_WEEK = {"", "일", "월", "화", "수", "목", "금", "토"};
        		final String [] AM_PM = {"오전", "오후"};
        		
        		return(cal.get(Calendar.YEAR) + "년 " + (cal.get(Calendar.MONTH)+1) + "월 " 
        		+ cal.get(Calendar.DATE) + "일 " + AM_PM[cal.get(Calendar.AM_PM)] + " " + cal.get(Calendar.HOUR) 
        		+ ":" + cal.get(Calendar.MINUTE) + ":" + cal.get(Calendar.SECOND) + "초 " 
        		+ DAY_OF_WEEK[cal.get(Calendar.DAY_OF_WEEK)] + "요일");
        	}
        }
        ```
        
- **GregorianCalendar으로 날짜 지정하기**
    - 첫번째 방법 : GregorianCalendar 객체 생성 후 **set 메서드** 이용하기
    - **Today 객체 생성**후 원하는 형식으로 출력하기 위해 **toString** 호출
    - 월은 **-1** 해줘야함
        
        ```java
        public class Today3 {
        	public static void main(String[] args) {
        		// 현재날짜와 시간으로 설정
        		GregorianCalendar g = new GregorianCalendar();
        		
        		//특정날짜로 변경  (-1 해줘야하는거 주의)
        		g.set(2022, 4, 2);
        		Today t3 = new Today(g);
        		System.out.println(t3.toString());
        ```
        
    - 두번째 방법 : **생성자 초기화** 할때 매개변수로 특정날짜 입력
        
        ```java
        		//생성자에서 년도, 달, 일 설정		
        		Today t1 = new Today(new GregorianCalendar(2021, 10, 30));
        		System.out.println(t1.toString());
        		
        		//생성자에서 년도,달,일,시,분,초 설정
        		Today t2 = new Today(new GregorianCalendar(2021,7,2,10,10,10));
        		System.out.println(t2.toString());
        ```
        
- **Timezone**
    - 다른 나라의 시간으로 바꾸고 싶을때 쓰임
    - 다른 나라의 **타임존, 아이디 조회**
    
    ```java
    GregorianCalendar cal = new GregorianCalendar();
    for (String name : TimeZone.getAvailableIDs())
    			System.out.println(name);
    ```
    
    - **다른 나라의 타임존 set**
        - **TimeZone 클래스**의 **getTimeZone 메서드**를 이용해 유럽 타임존을 불러온다
        - **calendar클래스**의 **setTimeZone 메서드**를 이용해서 calendar의 타임존을 바꾼다
        - **Today클래스**를 이용해서 출력
    
    ```java
    		GregorianCalendar calendar = new GregorianCalendar();
    		TimeZone timezone = TimeZone.getTimeZone("Europe/London");
    		calendar.setTimeZone(timezone);
    		
    		System.out.println("런던의 현재 시간입니다");
    		Today t = new Today(calendar);
    		System.out.println(t);
    ```
    
- **Date**
    - 현재 시간과 날짜를 구할 수 있음
        
        ```java
        		Date d = new Date();
        		System.out.println("Date 사용 :" + d);
        ```
        
- **SimpleDateFormat**
    - 원하는 포맷을 객체생성때 넣어서 **format메서드**로 호출
        
        ```java
        Date d = new Date();
        
        		SimpleDateFormat sd3 = new SimpleDateFormat("yyyy년 MM월 dd일 a h시 m분 s초 E요일");
        		System.out.println(sd3.format(d));
        ```
        
    - **calendar객체**가 가지고 있는 날짜와 시간과 **똑같은 정보를 갖는 Date 객체**를 만들어서 리턴하는 메서드 : **calendar.getTime()**
        - 왜냐하면 SimpleDateFormat은 **date형식**으로만 구동가능
        
        ```java
        GregorianCalendar calendar = new GregorianCalendar();
        				
        		System.out.println(calendar.getTime());  // Date객체가 되어버림
        		
        		SimpleDateFormat sdf = new SimpleDateFormat("yyyy년 MM월 dd일 a h시 m분 s초 E요일");
        		System.out.println(sdf.format(calendar.getTime()));
        	}
        ```
        
- **Random**
    - **Math.Random()**
        
        ```java
        for(int i=0; i<6; i++)
        		System.out.println((int)(Math.random()*45+1));
        ```
        
    - **Random 클래스**
        - 객체를 만들고 **nextInt** 메서드를 사용
        
        ```java
        Random random = new Random();
        for(int i=0; i<6; i++) {
        			System.out.println((random.nextInt(45)+1)+"\t");
        		}
        ```
        
- **Exception**
    - **finally**
        - 예외유무와 상관없이 쓸 수 있음
    - 부모 예외와 자식 예외중 **부모를 먼저 쓰면**  **“It is already handled by the catch block”** 이런 오류가 발생
        - 왜냐하면 부모가 자식을 포함하고 있기 때문에 부모를 통해 이미 자식의 예외 사항도 처리하게 됨(다형성)
        - 결국, IOException 하나만으로 예외를 잡을 수 있음
            
            ```java
            try {
            			FileReader reader = new FileReader("some.txt");  // i.o는 부모, filenotfound는 자식
            			reader.close();
            		} catch (IOException e) {	
            			System.out.println("입출력 에러가 발생했습니다");
            			
            			// 순서가 바뀌면 오류발생. 이미 부모 IOException에서 FileNotFoundException도 처리했기 때문에 (다형성) 이부분 필요 없음
            			
            		} catch (FileNotFoundException e) {    
            			System.out.println("파일을 찾을 수 없습니다");
            		}
            	}
            ```
            
    - **nullPointerException**
        - reader.close를 하려다 보니깐 null 상태라서 에러가 생김
        - null이면 close를 못하니깐 null이 아닐때 close를 할 수 있게 코드 추가
        - **if(reader ! = null) reader.close(); → 아직 잘 모르겠음**
            
            ```java
            FileReader reader =null; // 밖에서 선언해야 try-catch문에 안에서도 변수를 사용할 수 있음
            		try {
            			reader = new FileReader("some.txt");  // null 상태로 만들어졌기 때문에 nullpointerexception 에러
            		} catch (IOException e) {	
            			System.err.println("입출력 에러가 발생했습니다1");  // err로 바꾸면 빨간색으로 표시됨			
            		} finally {
            			try {
            				if (reader !=null)   // null이면 close를 못하니깐 
            					reader.close();
            			} catch (IOException e) {
            				System.out.println("파일 닫는 중 오류 발생했습니다");
            			}
            			
            		}
            ```
