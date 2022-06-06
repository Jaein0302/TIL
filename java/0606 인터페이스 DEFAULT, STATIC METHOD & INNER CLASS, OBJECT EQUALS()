- **인터페이스 DEFAULT, STATIC METHOD**
    - Default Method
        - 왜 쓰는가?
            - 인터페이스를 변경하게 되면 해당 인터페이스를 구현한 클래스도 변경해야하는 번거로움을 해결할 수 있다
        - 특징
            - **참조변수**로 호출 가능
    - Static Method
        - 왜 쓰는가?
            - 인스턴스 생성과 상관없이 인터페이스 타입으로 호출하는 메서드
        - 특징
            - 반드시 **클래스명**으로 메서드 호출
    
    ```java
    public interface Calculator {
        default int exec(int i, int j){      //default로 선언함으로 메소드를 구현할 수 있다.
            return i + j;
        }
        
        public static int exec2(int i, int j) {   //static 메소드
        	return i*j;
    	}
    }
    ```
    
- **인터페이스 내부클래스**
    - 왜 쓰는지?
        - 내부 클래스에서 외부 클래스의 멤버에 손쉽게 접근할 수 있게 됩니다.
        - 서로 관련 있는 클래스를 논리적으로 묶어서 표현함으로써, 코드의 **캡슐화**를 증가시킵니다
        - 외부에서는 내부 클래스에 접근할 수 없으므로, **코드의 복잡성을 줄일 수 있습니다.**
    - 종류
        - 인스턴스 클래스
            - 필드 선언하는 위치에 선언
            - **외부클래스 객체를 먼저 생성**한 후에 내부클래스 객체 생성 가능
            
            ```java
            public class InnerExam1 {
            	class Cal{			// 필드 선언하는 위치에 선언
            		int value = 0;
            		public void plus() {
            			value++;
            		}
            	}
            	
            	public static void main(String args[]) {
            		InnerExam1 t = new InnerExam1();
            		InnerExam1.Cal cal = t.new Cal();
            		cal.plus();
            		System.out.println(cal.value);
            	}
            }
            ```
            
        - 스태틱 클래스
            - 필드 선언하는 위치에 static으로 선언
            - **외부클래스 객체 생성없이** 내부클래스 생성 가능
            
            ```java
            public class InnerExam2 {
            	static class Cal{			// 내부 클래스가 static으로 정의 된 경우
            		int value = 0;
            		public void plus() {
            			value++;
            		}
            	}
            	
            	public static void main(String args[]) {
            		InnerExam2.Cal cal = new InnerExam2.Cal();		//InnerExam2객체 생성없이 바로 Cal 객체 생성 가능
            		cal.plus();
            		System.out.println(cal.value);
            	}
            }
            ```
            
        - 로컬 클래스
            - 외부 클래스 메서드 내부에 위치하는 클래스
            - 메서드를 호출하면 내부클래스의 메서드도 같이 수행
            
            ```java
            public class InnerExam3 {
            	public void exec() {
            		class Cal{				//메서드 안에 클래스 선언
            			int value = 0;
            			public void plus() {
            				value++;
            			}
            		}
            	
            		Cal cal = new Cal();
            		cal.plus();
            		System.out.println(cal.value);
            	}
            	
            	public static void main(String args[]) {
            		InnerExam3 t = new InnerExam3();		//InnerExam2객체 생성없이 바로 Cal 객체 생성 가능
            		t.exec();
            	}
            }
            ```
            
        - **익명 클래스 → 아직 잘모르겠음**
            - 클래스의 **선언과 객체의 생성을 동시**에 하기 때문에 **단 한번**만 사용될 수 있고 오직 하나의 객체만을 생성할 수 있는 **일회용 클래스**
            - 부모객체를 생성한 후 부모 추상클래스를 구현한다
            - 그리고나서 참조변수를 이용하여 호출
            
            ```java
            public abstract class Action {
            	public abstract void exec();	
            }
            ```
            
            ```java
            public class ActionExam {
            	public static void main(String args[]) {
            		Action action = new Action() {
            			public void exec() {
            				System.out.println("exec");
            			}
            		};
            		action.exec();
            	}
            }
            ```
            
- **OBJECT equals()**
    - 참조형 (String 제외)
        - 위는 false, 아래는 false
    
    ```java
      Fruit fruit1 = new Fruit("사과");
    	Fruit fruit2 = new Fruit("사과");
    	
    	System.out.println(fruit1.equals(fruit2)); // (1)
    	System.out.println(fruit1==fruit2);        // (2)
    ```
    
    - String
        - 위는 true, 아래는 false
    
    ```java
      String str3 = new String("abc");
    	String str4 = new String("abc");
    		
    	System.out.println(str3.equals(str4)); // (3)
    	System.out.println(str3==str4);        // (4)
    ```
    
    - equals()로 비교할때 왜 **참조형**과 **String**의 값이 다를까?
        - (1)과 (3)의 equals메서드가 서로 달라서 그렇다
        - 참조형의 경우에는 **overriding**해서 사용해야 한다
            - (1)의 경우 Fruit 클래스에 아직 implemented 되지 않아서 **Object의 equals()**를 사용하게 되는데 다음과 같다. (주소값을 비교하게된다)
                
                ```java
                public boolean equals(Object obj) {
                        return (this == obj);
                    }
                ```
                
            - (3)의 경우 String의 모든 char를 가지고 와서 비교한다
