- **쓰레드**
    - **쓰레드**
        - **프로세스 내**에서 실행되는 **세부 작업 단위**
    - **싱글 쓰레드**
        - 쓰레드가 하나뿐인 프로그램 (main 메서드 하나)
    - **멀티 쓰레드**
        - 쓰레드가 두개인 프로그램
        - 실행 중인 쓰레드가 하나라도 있다면 프로세스가 종료되지 않는다
        - 장점 (입출력 받는 동안 다른 작업을 할 수 있기 때문)
            - 데이터를 **입력**받는 작업
            - 네트워크로 파일을 **주고 받는** 작업
            - 프린터로 파일을 출력하는 작업 등 **외부기기와의 입출력**을 필요로 하는 경우입니다.
    - **프로세스**
        - 운영체제에서 실행 중인 **하나의 애플리케이션**
    - 쓰레드 생명주기
        - **New :** 스레드가 만들어진 상태
        - **Runnable :** 스레드 객체가 생성된 후에 start()호출하면 Runnable 상태로 이동
        - **Running :** Runnable 상태에서 스레드 스케줄러에 의해 Running 상태로 이동
        CPU를 점유하고 run() 메소드 내의 명령문을 실행하는 상태
        - **Blocked :** 특정 메소드의 호출에 의해서 현재 실행중인 Thread가 CPU의 제어권
        을 잃어버린 상태
        - **Dead :** run() 메소드의 명령 수행이 끝났을 경우
- **멀티쓰레드 작성 방법**
    - **java.lang.Thead** 클래스 이용 방법
        - **Thread 클래스**
            - **Thread로부터 상속**을 받고, **run()**을 통해 수행하고 싶은 일을 적는다
            - public class **DigitThread extends Thread**{
                public void **run()**{
                    for (int cnt = 0; cnt < 10; cnt++)
                        System.out.print(cnt);
                }
            }
        - **Main 클래스**
            - Thread 객체 생성 후 **start()**을 통해 쓰레드 실행
            - **Thread thread = new DigitThread();**
            **thread.start();**
    - **java.lang.Runnable** 인터페이스 이용 방법
        - **Runnable 인터페이스 구현**
            - **Runnable 인터페이스를 구현**시킨 다음 **run()**에 수행하고 싶은 일을 적는다
            - public class SmallLetters **implements Runnable** {
                public void **run()** {
                    for (char ch = 'a'; ch <= 'z'; ch++)
                        System.out.print(ch);
                }
            }
        - **Main 클래스**
            - **SmallLetters 객체** 생성 ⇒ **Thread 객체** 생성
            - Thread 객체 생성할때 **small (SmallLetters의 참조변수)**를  매개로 **Thread생성자 호출**
            - **SmallLetters** small = new SmallLetters();
            **Thread** thread = new Thread(**small**);  →   Thread thread = new Thread(**new SmallLetters()**); 와 같음
            thread.**start();**
    - **Runnable 익명클래스**
        - 왜 쓰는지?
            - Thread class는 다중상속이 불가능하기 때문에 **다중상속**이 필요한 경우 interface를 활용함
            - 클래스를 안나누고 main에 쓰레드를 **하나로** 합칠때 사용 (쓰레드를 일시적으로 사용할때)
        - Thread 객체 생성할때 **new Runnable(){}**를 매개로 Thread 생성자 호출 (객체 생성 X)
        - **{ } 안**에 run() 메서드를 넣는다
            - Thread thread = new Thread(**new Runnable() {**
                public void run() {
                    for (char ch = 'a'; ch <= 'z'; ch++)
                        System.out.print(ch);
            }
            **});**
- **Thread.sleep()**
    - **일정 시간이 경과**되기를 기다리는 메서드
    - InterruptedException를 처리하는 예외 추가
        
        ```java
        public class DigitThread extends Thread	{
        	public void run() {
        		for (int cnt=0; cnt<10; cnt++) {
        			System.out.println(cnt);
        			try {
        				Thread.sleep(1000); //1초
        			} catch (InterruptedException e) {
        				System.out.println(e.getMessage());
        			}
        		}
        	}
        }
        ```
        
    - **매 시각을 출력하는 Thread**
        
        ```java
        public class ThreadSleep_date extends Thread {
        		public void run() {
        			SimpleDateFormat sd3 = new SimpleDateFormat("yyyy년 MM월 dd일 E요일 h시 m분 s초 ");
        			while(true) {
        				Date d = new Date();
        				System.out.println(sd3.format(d));
        			try {
        				Thread.sleep(1000); //1초
        			} catch (InterruptedException e) {
        			//sleep 메서드가 발생시키는 InterruptedException 처리하는 문장
        				System.out.println(e.getMessage());
        			}
        		}
        	}
        }
        ```
        
- **Daemon Thread**
    - 다른 일반 쓰레드(데몬 쓰레드가 아닌 쓰레드)의 작업을 돕는 **보조적인 역할**을 수행하는 쓰레드
    - **일반 쓰레드가 모두 종료**되면 **데몬 쓰레드는 강제적으로 자동 종료**
    - **쓰레드** 생성 -> 반드시 **setDaemon(true)메서드** 호출 -> **start() 메서드** 호출
    - void setDaemon (boolean on) : **true (데몬 쓰레드로 설정)**, **false(일반 쓰레드로 설정)**
    - **데몬 쓰레드**
        - 3초 후에 autosave() 메서드가 수행되어야 하지만,
        - main에서 **autoSave가 true** 되어야지만 **autoSave() 메서드**가 수행
        
        ```java
        public class Threadautosave implements Runnable {
        	static boolean autoSave = false;
        	public void run() {
        		while (true) {
        			try {
        				Thread.sleep(3*1000);
        			} catch (InterruptedException e) {
        				e.printStackTrace();
        			}
        			//autosave의 값이 true이면 autosave()를 호출한다
        			if(autoSave) {
        				autoSave();
        			}
        		}
        	}
        	public void autoSave() {
        		System.out.println("작업파일이 자동저장되었습니다");
        	}
        }
        ```
        
    - **Main**
        - setDaemon(true)
            - Thread t = new Thread(**new Threadautosave()**);
                - Threadautosave **th** = new Threadautosave();
                Thread t = new Thread(**th**);    와 같음
            - **데몬쓰레드**로 만들어줌
            - 보조 쓰레드이기 때문에 **Main에서 수행이 끝나면** 자동으로 **끝날 수 있게** 해줌
                
                ```java
                public static void main(String[] args) {
                //		Threadautosave th = new Threadautosave();
                //		Thread t = new Thread(th); 와 같음
                		
                		Thread t = new Thread(new Threadautosave()); 
                		
                		t.setDaemon(true); // 이 부분이 없으면 종료되지 않습니다.
                							// 데몬 스레드로 만듭니다.
                		t.start();
                		
                		for (int i=1; i<=30; i++) {
                			try {
                				Thread.sleep(1000);
                			} catch (InterruptedException e) {
                				System.out.println(e.getMessage());
                			}
                			System.out.println(i);
                			
                			if(i==5)
                				Threadautosave.autoSave = true;
                		}
                		System.out.println("프로그램을 종료합니다.");
                	}
                ```
                
- **공유영역 만들기**
    - SharedArea
        - **getter, setter**를 이용하여 불러오고 수정하는 것이 가능하도록 함
        - **setReady()**를 통해 isReady가 **True**가 되면  **isReady()**를 통해 **True**가 리턴되도록 장치 설정
        
        ```java
        public class SharedArea {
        	private double result;
        	
        	SharedArea(){};
        	
        	public double getResult() {
        		return result;
        	}
        
        	public void setResult(double result) {
        		this.result = result;
        	}
        
        	private boolean isReady;  // 기본값false
        	
        	public boolean isReady() {
        		return isReady;
        	}
        	
        	public void setReady(boolean isReady) {
        		this.isReady = isReady;
        	}
        }
        ```
        
    - Main
        - **SharedArea 객체** 생성
        - **SharedArea 참조값**을 CalcThread, PrintThread의 **필드에 저장**
        
        ```java
        public class MultithreadExample {
        
        	public static void main(String[] args) {
        		SharedArea obj = new SharedArea();
        		CalcThread thread1 = new CalcThread(obj);
        		PrintThread thread2 = new PrintThread(obj);
        		
        		thread1.start();
        		thread2.start();
        	}
        }
        ```
        
    - CalcThread
        - 계산을 하는 run() 생성
        - **SharedArea 참조값이 필드에 저장**되어 있기 때문에 SharedArea 클래스에 있는 **setResult에 저장 가능**
        - 계산이 끝나면 setReady()를 통해 **isReady를 True**로 바꾼다
        
        ```java
        public class CalcThread extends Thread {  
        	private SharedArea sharedarea;
        	
        	CalcThread(SharedArea sharedarea) {
        		this.sharedarea = sharedarea;
        	}
        	
        	public void run() {
        		double total = 0.0;
        		for (int cnt=1; cnt<1000000000; cnt+=2)
        			if (cnt / 2 % 2 ==0)
        				total += 1.0 / cnt;
        			else
        				total -= 1.0 /cnt;
        		
        		sharedarea.setResult(total*4);   
        		sharedarea.setReady(true);
        	}
        }
        ```
        
    - PrintThread
        - **SharedArea 참조값이 필드에 저장**되어 있기 때문에 SharedArea 로부터 값을 가져오는 **getResult 사용가능**
        - isReady가 True가 되기 전까지 getResult()를 실행 안하고 **True 되면 실행**
        
        ```java
        public class PrintThread extends Thread	{
        	private SharedArea sharedarea;
        	
        	PrintThread(SharedArea sharedarea) {
        		this.sharedarea = sharedarea;
        	}
        	
        	public void run() {
        		while (sharedarea.isReady() != true) {
        			System.out.println("실행중");
        		}
        		System.out.println("공유 영역의 데이터 =" + sharedarea.getResult());
        	}
        }
        ```
