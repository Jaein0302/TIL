# 0613

- **동기화**
    - **공유데이터 사용 중**에 그 공유 데이터를 **다른 스레드가 사용하지 못하도록** 만드는 것
    - 두 계좌에서 서로 **인출&입금**과 **계좌 잔액**을 출력하는 두개의 스레드가 있다
    - 인출&입금 → 계좌 잔액 출력의 순서가 되려면 **동기화**가 필요하다
        - **Main**
            - **Account**의 두계좌 **참조 변수 a1, a2**를 공유영역 **SharedArea의 필드**로 저장
            - SharedArea의 참조변수 area를 Thread1의 필드로 저장
            - SharedArea의 참조변수 area를 Thread2의 필드로 저장
        - **Account**
            - accountno, ownername, balance를 필드로 저장하고
            - getter, setter 메서드 + deposit, withdraw 메서드를 사용한다
        - **SharedArea**
            - account1, account2를 필드로 저장하고
            - 각 account의 getter,setter메서드
        - **PrintThread**
            - 해당하는 Thread 수행부분을 synchronized로 감싸서 **동기화**시킨다 (동기화 블럭)
            - **synchronized (sharedarea) { }**
                - (  ) 안에 들어오는 매개변수는 **공유 객체**를 적어주면 된다.
                - {  } 안에는 수행할 문장을 적어주면 된다
            
            ```java
            	public void run() {
            		for(int i=0; i<3; i++) {
            			// synchronized 키워드를 사용해서 블록으로 묶은 부분을 동기화 블럭이라고 한다
            			synchronized (sharedarea) {
            			int sum = sharedarea.getAccount1().getBalance() + sharedarea.getAccount2().getBalance();
            			System.out.println("계좌 잔액 합계:" + sum);
            			}			
            		try {
            			Thread.sleep(1);
            		} catch (InterruptedException e) {
            			System.out.println(e.getMessage());
            		}
            	}
            }
            ```
            
            ```java
            public void run() {
            		for(int i=0; i<12; i++) {
            			//동기화 시작
            			synchronized (sharedarea) {
            				sharedarea.getAccount1(). withdraw(1000000); 
            				System.out.print("이몽룡 계좌: 100만원 인출,");
            				sharedarea.getAccount2(). deposit(1000000); 
            				System.out.println("성춘향 계좌: 100만원 입금");
            			}
            		}
            	}
            ```
            
- **동기화 출력**
    - **동기화 블럭**
        - 일부분만 임계 영역으로 만들 경우
            
            ```java
            int getTotal() { 
            		synchronized (this)	 {
            			return account1.getBalance() + account2.getBalance();
            		}
            	}
            	
            public Account getAccount1() {
            	 return account1;
            ```
            
    - **동기화 메서드**
        - 메서드 전체 내용을 임계 영역으로 만들 경우
            
            ```java
            synchronized void transfer() {	
            			account1.withdraw(1000000);
            			account2.deposit(1000000);
            	}
            
            synchronized int getTotal() { 
            			return account1.getBalance() + account2.getBalance();
            	}
            ```
            
- **Notify_Wait**
    - 동기화(Synchronized)된 블록에서 **스레드간의 통신**(제어권을 넘김)하기 위해서 notify, notifyall, wait 메서드를 사용한다
    - **계산하는 스레드(Calc)**가 끝난다음에 **프린트하는 스레드(Print**를 생성하고 싶다면
    - **Notify**로 신호를 보내고, 상대방은 그 스레드가 끝날때까지 **Wait**하면 된다
        - CalcThread
            
            ```java
            public void run() {
            		double total = 0.0;
            		//파이를 계산하는 부분
            		for (int cnt=1; cnt<1000000000; cnt+=2)
            			if (cnt / 2 % 2 ==0)
            				total += 1.0 / cnt;
            			else
            				total -= 1.0 /cnt;
            		
            		sharedarea.setResult(total*4);   
            		
            		synchronized (sharedarea) {
            			sharedarea.notify();  // 다른 스레드로 신호를 보내는 메서드
            		}
            ```
            
        - PrintThread
            
            ```java
            public void run() {
            			try {
            				synchronized (sharedarea) {
            					sharedarea.wait();  // 다른 스레드로부터 신호를 기다린다. 
            										// 파이의 계산이 끝나면 다른 스레드에서 신호를 보낸다
            				}
            			}
            			catch (InterruptedException e) {  // wait 메서드가 발생하는 인셉션 처리
            				System.out.println(e.getMessage());
            			}			
            		
            		System.out.println("공유 영역의 데이터 =" + sharedarea.getResult());
            	}
            ```
            
- NotifyAll_Wait
    - 만약 3개이상의 스레드일때 **notifyAll**를 쓰면 기다리고 있던 모든 스레드에게 신호를 보낸다
    - 3개 이상의 스레드인데 **notify**를 쓰면 랜덤으로 가장 빠른 하나의 스레드에게만 신호를 보냄
- **Stream**
    - 데이터를 **운반(입출력)**하는데 사용되는 연결통로
    - **일차원적인 데이터**의 흐름
    - 자바 프로그램에서는 입력되고 출력되는 **모든 데이터**를 스트림 형태로 주고 받는다
    - **흐름의 방향**에 따른 종류
        - 입력 스트림 : 프로그램으로 들어오는 스트림
        - 출력 스트림 : 프로그램 밖으로 나가는 스트림
    - **데이터의 형태**에 따른 분류
        - **문자** 스트림 : **2바이트** 단위로 읽고 쓰는 스트림
            - **Reader, Writer**
        - **바이트** 스트림 : **1바이트** 단위로 읽고 쓰는 스트림
            - **InputStream, OutputStream**
    - **파일 입출력 과정**
        - 파일을 연다
        - 데이터를 읽거나 쓴다
        - 파일을 닫는다
- **FileReader**
    - **파일을 연다**
        - **상대경로**
            - **workspace project 폴더**를 기준으로 위치 설정
            - 그 위치를 **FileReader의 생성자 호출**에 넣어주면 됨
                
                **FileReader** reader = **new FileReader** ("poem.txt");
                
        - **절대경로**
            - FileReader reader = new FileReader("C:/java_국비/workspace/java/poem.txt");
            - FileReader reader = new FileReader("C:\\java_국비\\workspace\\java\\poem.txt");
    - **파일을 읽는다 (하나씩 읽어오기)**
        - **read**는 파일에 있는 **문자 하나(2바이트)**를 읽어서 리턴하는 메서드
        - **data = -1**은 더이상 읽을 데이터가 없는 경우
            - while(true) {
                
                    int data = **reader.read();**
                
                    if(**data == -1**)
                
                        break;
                
                    char ch = **(char) data;**
                
                    System.out.print(ch);
                
                }
                
        - **FileNotFoundException**과 **IOException**이 발생하기 때문에 위의 열고 읽는 문장을 try-catch문으로 감싼다
            - **try** {
                
                    여는 문장
                
                    읽는 문장
                
                catch **(FileNotFoundException fnfe)** {		
                    System.out.println("파일이 존재하지 않습니다");
                }
                
                catch **(IOException e)** {
                	System.out.println("파일을 읽을 수 없습니다");
                }
                
    - **파일을 읽는다 (배열로 읽어오기)**
        - **char arr[] = new char[64]**를 통해 파일에 있는 문자를 담을 char배열 생성
        - **reader.read(arr)**는 **arr**에 배열크기만큼 문자를 **저장**하고, arr의 **배열 크기**를  **return**한다
            - **chat arr[] = new char[64];**
                
                try {
                
                reader = new FileReader (”거위의 꿈.txt”);
                
                while(true) {
                
                int num = **reader.read(arr);**
                
                if (num == -1)
                
                break;
                
                for (int cnt=0; cnt<**num**; cnt++) {
                
                System.out.printf(”%c”, arr[cnt]);
                
                }
                
        - String 메서드의 생성자 호출을 이용하여 출력할 수 있다
        - 원래는 **String s = new String(arr,0,num);** 에서 **s.toString**을 출력하는 것과 같다
            - System.out.println(**new String(arr, 0, num)**);
    - **파일을 닫는다 (예외처리)**
        - 닫는 문장은 try-catch문장 안에 넣지 않는다. 왜냐하면 read() 메서드에서 오류가 발생한다면 바로 try-catch문으로 넘어가서 **close()메서드가 실행되지 않은채 프로그램이 끝난다**
        - 이것을 해결하기 위해 close()메서드를 **finally 블록**에서 사용
            - **reader 변수**를 finally문에서 사용하기 위해 **try문 전**에 선언
                - FileReader reader = null;
                    
                    try {
                    
                    열고 읽는 문장
                    
                    } catch
                    
    - **파일을 닫는다 (예외처리1)**
        - close() 메서드에서는 파일이 null일때 **NullPointerExceptoin**이 발생한다
            - if절로 **null값이 아닌 경우에만 close**를 시킨다
                - finally {
                    **try** {
                        **if(reader != null)**
                            **reader.close();**
                    } **catch** (IOException e) {
                        System.out.println("IOException");
                    }
    - **파일을 닫는다 (예외처리2)**
        - 상위 예외인 **Exception**으로 try-catch을 통해 잡는다
            - finally {
                **try** {
                    reader.close();
                } **catch (Exception e)** {
                    System.out.println("Exception");
                }
    - **파일을 닫는다** **(예외처리3)**
        - **Try-with-resource**를 사용하면 close메서드를 자동으로 호출한다
        - FileReader 선언문을 try ( ) 괄호안에 넣어주면 된다.
            - **try (**
                
                    **FileReader reader = new FileReader ("거위의 꿈.txt");**		
                **)** {
                
                읽는 문장
                
                } catch
                
- **FileWriter**
    - **파일에 쓰기**
        - Preference에서 Refresh using native hooks or polling E 설정해줘야 자바에서 볼 수 있음
        - 생성자의 매개변수가 **열 파일의 이름**을 의미
        - FileWriter 생성자는 IOException에러 처리문 필요
        - for(int cnt=0; cnt<arr.length; cnt++)
            writer.write(arr[cnt]);    =   **writer.write(arr);  (같은 문장)**
            - FileWriter writer = null;
            **try {**
                writer = **new FileWriter("output.txt");**
                **char arr[]** = {'내', '꺼', '인', ' ', '듯'}
                **writer.write(arr);**
            } **catch** (IOException ioe) {
                System.out.println("파일로 출력할 수 없습니다");
            } **finally** {
                    try {
                        writer.close();
                    }
                    catch(Exception e) {
                        System.out.println("파일 닫는 중 오류입니다");
            	}
    - **파일 추가로 쓰기 & 덮어쓰기**
        - **true** : append(원래 있던 파일 내용 뒤에 추가로 쓴다)
        **false** : overwrite(덮어쓰기 한다)
        - try {
            
                writer = **new FileWriter("output.txt", true**);
            
                **char arr[]** = {'너', '를', ' ', '사', '랑', '해'};
            
                **writer.write(arr);  →**  원래 있던 파일에 추가로 쓴다
            
- **FileInputStream**
    - Run configurations에서 **argument값**에 열고 싶은 “**파일명” 입력 (자바 폴더에 있어야함)**
    - 데이터는 바이트 단위이기 떄문에 **byte[]** 배열을 만들어준다
        - FileInputStream in = null;
            
            **try** {
            
            in = **new FileInputStream (args[0]);**
            
            **byte arr[] = new byte[21];**
            
            while (true) {
            
            int num = in.**read(arr);**
            
            if(num < 0)
            
                break;
            
            System.out.println("\n16진수 출력");
            for (int cnt=0; cnt< **num**; cnt++)
            System.out.printf("%02X ", **arr[cnt]**);
            
            }
            
- **FileOutputStream**
    - FileOutputStream out = null;
    try {
        out = **new FileOutputStream("output1.dat");**
        byte arr[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    
    	    for(int cnt=0; cnt<arr.length; cnt++)
    		**out.write(arr[cnt]);**
    	}
    
- **데이터 복사하기**
    - 복사대상을 read하고, 복사할 곳에 write하면 된다
    - **fis.read(readBytes)**는 복사 대상을 readBytes라는 배열에 저장하기 때문에 **fos.write(readBytes)**에서 대상이 저장된 배열인 readBytes를 생성자 호출해야 한다.
    - String originalFileName = "C:/temp/koala.jpg";
    String targetFileName = "C:/temp2/koala_copy.jpg";
    FileInputStream fis = null;
    FileOutputStream fos = null;
    try {
        fis = new FileInputStream(originalFileName);
        fos = new FileOutputStream(targetFileName);
        
        
            int readCount;
            **byte[] readBytes = new byte[1024];**
            
        
            while(true) {
        	readCount = **fis.read(readBytes);**
        			if(readCount == -1)
        				break;
        			**fos.write(readBytes);**
            }
        
- **Buffered**
    - Buffer 사용 이유?
        - 입출력 소스와 직접 작업하지 않고 buffer와 작업하여 실행 성능을 향상시킨다
    - **BufferedReader**
        - FileReader와 흐름은 비슷하고, BufferedReader 생성자 매개변수에 FileReader의 객체를 넣으면 된다 (FileReader를 BufferedReader에서 필드로 사용하겠다라는 의미)
        - FileReader f = **new FileReader("src/ex18_4_Buffered/output2.dat")**
            
             reader = **new BufferedReader (f);   →**   reader = **new BufferedReader (new FileReader("src/ex18_4_Buffered/output2.dat")); 와 같다**
            
        - while (true) {
            
            if (num == -1)
            
            break;    =   **while((num=reader.read(arr)) != -1)  와 같다**
            
        - new String(arr,0,num) **= new String(arr) 와 같다**
            - **BufferedReader reader = null;**
            char arr[] = new char[64];
            int num =-1;
            try {
                reader = **new BufferedReader (new FileReader("src/ex18_4_Buffered/output2.dat"));**
            while**((num=reader.read(arr)) != -1)** {
            	        **System.out.print(new String(arr,0,num));**
            		}
    - **BufferedWrite**
        - char, byte 배열이 아닌 **String**으로 write할 수 있음
        - **String message** = "하하입니다. \n 오늘도 화이팅입니다";
        
        	BufferedWriter writer = null;
        	try {
        		writer = **new BufferedWriter(new FileWriter("src/ex18_4_Buffered/output2.dat"))**;
        		writer.write(**message**);
        
         }
        

****
