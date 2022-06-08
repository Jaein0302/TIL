# 0607

- **과제리뷰**
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
        - **GregorianCalendar**는 Calendar를 **상속 받아 구현한것**
        
        ```java
        Calendar cal = new GregorianCalendar();		
        		cal.get(Calendar.YEAR) + "년 " + (cal.get(Calendar.MONTH)+1) + "월 ";
        ```
        
    - **GregorianCalendar g = new GregorianCalendar();**
        - **GregorianCalendar 객체**를 만들어서 사용해도 가능
        
        ```java
        GregorianCalendar c = new GregorianCalendar();
        		
        		c.add(Calendar.DATE, 1);
        		System.out.println("1 증가된 날 =" + c.get(Calendar.DATE));
        		print(c);
        ```
        
        - 부모 클래스에 있던 **생성자를 호출하여 활용**
        
        ```java
        Today(GregorianCalendar cal) {
        		this.cal = cal;
        	}
        ```
        
        ```java
        GregorianCalendar g = new GregorianCalendar();
        		
        		//특정날짜로 변경  (-1 해줘야하는거 주의)
        		g.set(2022, 4, 2);
        		Today t3 = new Today(g);
        		System.out.println(t3.toString());
        		
        		//생성자에서 년도, 달, 일 설정
        		Today t1 = new Today(new GregorianCalendar(2021, 10, 30));
        		System.out.println(t1.toString());
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
        
    - **calendar객체**가 가지고 있는 날짜와 시간과 **똑같은 정보를 갖는 Date 객체**를 만들어서 리턴하는 메서드
        
        ```java
        GregorianCalendar calendar = new GregorianCalendar();
        				
        		System.out.println(calendar.getTime());  // Date객체가 되어버림
        		
        		SimpleDateFormat sdf = new SimpleDateFormat("yyyy년 MM월 dd일 a h시 m분 s초 E요일");
        		System.out.println(sdf.format(calendar.getTime()));
        	}
        ```
        
- **Random**
    - Math.Random()
        
        ```java
        for(int i=0; i<6; i++)
        		System.out.println((int)(Math.random()*45+1));
        ```
        
    - Random 객체
        
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
