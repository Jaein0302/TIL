
- **다형성**
    - 자바는 한 타입의 참조변수로 여러타입의 객체를 참조할 수 있도록 했는데 **조상 클래스 타입의 참조변수**로 **자손 클래스 인스턴스를 참조**할 수 있도록 하였습니다
    - 상속관계에 있을때 **메서드를 하나**만 이용하여 호출하고 싶을때 쓰임
    - 이때 **부모클래스를 매개변수**로 사용하면 상속관계 있는 자손클래스들을 메소드로 호출할 수 있다. **(UP-CASTING)**
    - 단, **부모**클래스에 있는 **필드, 메서드만** 호출 가능하다 (범위 축소)
        - 만약 부모클래스에 메서드가 **추상메서드**라면 자손클래스에서 **오버라이딩한 메서드**로 넘어간다.
        - 부모클래스에 있는 메서드가 **추상메서드가 아니라면 에러**
            - 부모클래스인 MessageSender의 참조변수로 메서드 호출
                
                ```java
                public static void main(String[] args) {
                		EmailSender obj1 = new EmailSender ("생일축하", "고객센터", "jaein@naver.com", "할인쿠폰");
                		SMSSender obj2 = new SMSSender("생일축하", "고객센터", "02-645-3158", "할인쿠폰");
                
                		send(obj1, "hoho@naver.com");
                		send(obj2, "010-5555-3158");
                	}
                
                	static void send(MessageSender obj, String recipient) {  // upcasting
                		obj.sendMessage(recipient);	
                	}
                ```
                
            - 부모클래스인 Lendable의 참조변수로 메서드 호출
                
                ```java
                public static void main(String[] args) {
                		Lendable arr0, arr1, arr2;
                		
                		arr0 = new SeparateVolume("883o", "푸코의 진자", "에코");
                		arr1 = new SeparateVolume("609.2", "서양미술사", "곰브리치");
                		arr2 = new AppCDInfo("02-17", "마이크로서비스를 위한 자바 프로그래밍");
                		
                		checkOut(arr0, "수지", "2022-06-02");
                		checkOut(arr1, "수지", "2022-06-02");
                		checkOut(arr2, "수지", "2022-06-02");
                	}
                	
                	static void checkOut(Lendable arr, String borrower, String date) {
                		arr.checkOut(borrower, date);
                	}
                ```
                
            - 부모클래스인 Lendable의 참고변수로 메서드 호출 → 각 주소값을 **객체배열**로 만들어서 호출 → 향상된 for문 사용
                
                ```java
                public static void main(String[] args) {
                		Lendable arr[] = new Lendable [3];
                		
                		arr[0] = new SeparateVolume("883o", "푸코의 진자", "에코");
                		arr[1] = new SeparateVolume("609.2", "서양미술사", "곰브리치");
                		arr[2] = new AppCDInfo("02-17", "XML를 위한 자바 프로그래밍", "유시진");
                		
                		checkOutAll(arr, "수지", "2022-06-02");
                	}
                	
                	static void checkOutAll(Lendable[] arr, String borrower, String date) {
                		for(Lendable arrs : arr)
                		arrs.checkOut(borrower, date);
                	}
                ```
                
- **다형성 실행하는 방법**
    - MessageSender obj : 슈퍼 클래스 타입의 파라미터
    - MessageSender 클래스에서 sendMessage 메소드를 주석 처리할 경우
    에러 발생합니다.
    - **자바 컴파일러**는 **부모 변수의 타입으로 그 메소드가 있는지 없는지 체크**합니다.
    MessageSender에 sendMessage()메서드가 존재하지 않아서 The method sendMessage(String) is undefined for the type 에러 발생했습니다.
    - **자바 가상 기계(JVM)**는 객체의 메소드를 호출할 때 변수의 타입에 상관없이
    **객체가 속하는 클래스의 메소드를 호출**합니다.
- **인터페이스와 예외처리**
    - 부모와 달리 **자식만 예외발생**하는 경우 또는 부모보다 자식의 **예외발생의 범위가 큰 경우**에는 오류가 발생한다.
        - 자식클래스에 **throws exception**을 썼으면 부모에게도 써줘야함
        - IOException이 Exception의 자식이므로 관계를 잘보고 써야함
- **참조형 NULL**
    - 주소값이 초기화가 아니고 null썼다는건 **누군가를 가르켜주는게 없다는 뜻** - runtime error
    - 자바의 정석 다형성이랑 같이 풀어나가면 될듯
- **UPCASTING, DOWNCASTING**
    - upcasting
        - 부모클래스의 참조변수로 변형
        - **Account** obj1 = new CheckingAccount("111-22-33333333", "홍길동", 10, "4444-5555-6666-7777");   // 업캐스팅
    - downcasting
        - upcasting한 객체만 downcasting이 가능하다 (처음투터 downcasting하는것은 안됨) - 복구의 의미
        - CheckingAccount obj2 = **(CheckingAccount)** obj1;  //  다운캐스팅
