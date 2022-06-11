# 0609

- **Wrapper Class**
    - 기본형 값을 **객체로 변환**시킨 클래스
    - 8가지의 래퍼 클래스가 존재 (기본형)
        - Boolean
        - Character
        - Byte
        - Short
        - Integer
        - Long
        - Float
        - Double
    - **똑같은 기본값**을 가진 래퍼 클래스라면 **참조값도 같다**
        - Integer obj1 = Integer.valueOf(10);
        Integer obj2 = Integer.valueOf(10);
        - **obj1 == obj2**  → 참조값이 같게 나옴
    - String과 마찬가지로 equals의 메소드는 **내용을 비교**하도록 오버라이딩 됨
        - **obj1.equals(obj2)** → 같게 나옴
- **박싱 & 언박싱**
    - **int → Integer (박싱)**
        - **Integer** obj = **Integer.valueOf**(12)
        - **Integer** obj = Integer.**valueOf(”20”);  → 정확히 말하면 박싱은 아님 (문자열이 기본형은 아니라서)**
    - **Integer → int (언박싱)**
        - int n = obj**.intValue();**
    - **String → int**
        - String → Integer → int (추천하지 않음)
            - for (int cnt = 0; cnt<args.length; cnt++) {
                **Integer** obj = **Integer.valueOf**(args [cnt]);
                total += obj.**intValue(**);
                }
        - String → int (추천방법)
            - for (int cnt = 0; cnt<args.length; cnt++) {
                total = **Integer.parseInt**(args [cnt]);
    - **int → String**
        - **Integer.toString**(10);  → 첫번째 방법
        - 10 + **“ “   →** 두번째 방법
    - **int → 진수**
        - Integer.**toBinaryString(num)) → 2진법**
        - Integer.**toOctalString(num)) → 8진법**
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2ecc94a3-0946-4cd7-9fdd-03e1f2d190d4/Untitled.png)
            
- **오토언박싱**
    - 객체 → 기본형 **자동으로 언박싱**
    - **Integer** obj = Integer.valueOf(”10”);
        
        int sum = obj + 20;  → Integer타입이 **자동으로 int로 변환**
        
        String a = obj + “” + 20;  →  Integer타입이 **자동으로 String으로 변환**
        
- **오토박싱**
    - printDouble (123.45)  → 자동으로 **Double.valueOf (123.45D)** 로 변환됨
        
        static void printDouble (Double obj) {
            System.out.println(obj);
            System.out.println(obj.doubleValue()); 
            System.out.println(obj.toString());
            }
        
- **컬렉션 프레임워크**
    - 쓰는 이유?
        - 배열은 불특정 다수의 객체를 저장하기 문제
        - 배열은 객체 삭제했을때 해당 인덱스가 비게 됨
- **List 컬렉션**
    - 특징
        - **객체**를 ****인덱스로 관리
        - 객체 자체를 저장하는 것이 아니라 **객체의 주소값**을 참조
        - **중복 저장** 가능 - 동일 한 주소값이 참조
    - 종류
        - **ArrayList**
            - **특징**
                - 기본 **10개**의 객체를 담을 수 있다
                - **입력한 순서대로** 출력
                - **접근시간**이 LinkedList보다 빠르다
                - **순차적으로 데이터를 추가, 삭제**시 LinkedList보다 빠르다
            - **List** list = new **ArrayList();**  = ArrayList list = new ArrayList();  (업캐스팅한 것뿐 같음)
            - **출력방법**
                - for(int cnt=0; cnt<num; cnt++) {
                    **String** str = **list.get(cnt);  →  index위치에 있는 자료를 String으로 가져온다**
                    System.out.println(str);
                }
                - **for (String str : list)** {   →  String으로 변환해서 출력
                    System.out.print(str);
                }
                - **Iterator**<Integer> iterator = **list.iterator();**
                while(iterator.**hasNext()**) {
                    System.out.println(iterator.next()+ "\t");
                }
            - **list.add(”포도”);**  → list에 추가
            - **list.set(1, ”포도");**  → 해당 인덱스에 있는 자료를 변경
            - **list.size();**  →  list에 있는 데이터의 수를 구하는 메서드
            - **get(index)**  →  list에 있는 자료 가져오
            - **list.remove(”키위”);**  →  같은 값이 있더라도 가장 빠른 index의 키위만 제거
            - **list.IndexOf(”사과”);**  →  앞에서부터 찾음
            - **list.lastIndexOf(”사과”);**  →  뒤에서부터 찾음
            - **list1.contains(list2.get(i));** →  list2.get(i)가 list1에 포함되어 있는지 확인
            - **list2.removeAll(list1);**  →  list2에서 list과 공통된 부분 제거
            - **객체배열**을 ArrayList로 구현하기
                - **main**
                    - **for**(Student2 s : students) {
                        **as.add(s)**;  		
                    }
                        - 객체배열을 **향상된 for문**으로 Student객체를 추출한다음
                        - ArrayList의 **add() 메서드**를 통해 Student객체를 하나씩 넣는다 (객체도 저장이 가능하다)
                    - **Student2.sort(as)**
                        - sort()와 printInfo() 메서드는 static으로 객체생성없이 호출이 가능하다
                        - ArrayList의 참조변수인 as를 호출하면 저장된 ArrayList를 매개변수로 메서드를 수행할 수 있다
                            
                            ```java
                            public static void main(String args[]) {		
                            		
                            		Student2[] students = { 
                            						new Student2("강호동",85,60,70),
                            						new Student2("이승기",90,95,80),
                            						new Student2("유재석",75,80,100),
                            						new Student2("하하",80,70,95),
                            						new Student2("이광수",100,65,80)};
                            		
                            		ArrayList<Student2> as = new ArrayList<>();
                            		for(Student2 s : students) {
                            			as.add(s);  		
                            		}
                            	
                            		Student2.sort(as);
                            		Student2.printInfo(as);
                            					
                            	}
                            ```
                            
                - **sort()**
                    - **as.get(i).getTotal**
                        - as.get(i)는 해당 Student 객체를 불러오는 것이고, getTotal를 붙임으로써 Student내에 있는 메서드를 호출하는 것이다.
                    - **Student2 imsi = as.get(i);**
                        - imsi라는 Student객체와 as.get(i)라는 Student객체를 서로 교환해주는 작업
                            
                            ```java
                            static void sort(ArrayList<Student2> as) {	
                            		for (int i = 0; i < as.size() - 1; i++) {
                            			for (int j = i + 1; j < as.size(); j++) {
                            				if (as.get(i).getTotal() < as.get(j).getTotal()) {
                            					Student2 imsi = as.get(i);
                            					as.set(i, as.get(j));
                            					as.set(j, imsi);
                            				}
                            			}
                            		}
                            	}
                            ```
                            
                - **PrintInfo()**
                    - **for(Student2 obj : objs)** {
                        System.out.print(obj.name+"\t"+obj.kor+"\t"+obj.eng+"\t"+obj.math+"\t"+obj.getTotal()+"\t"); }
                        - objs는 ArrayList의 매개변수이다. ArrayList에 구성되어 있는 Students 객체를 추출하기 위해 향상된 for문을 쓰고 거기서 나온 Student 타입의 참조변수로 다른 필드와 메서드를 호출한다
                            
                            ```java
                            static void printInfo(ArrayList<Student2> objs) {
                            		int total[] = new int[3];
                            
                            		for(Student2 obj : objs) {
                            			System.out.print(obj.name+"\t"+obj.kor+"\t"+obj.eng+"\t"+obj.math+"\t"+obj.getTotal()+"\t");
                            			System.out.printf("%.1f\n",obj.getAverage());
                            			
                            			total[0] += obj.kor;
                            			total[1] += obj.eng;
                            			total[2] += obj.math;
                            		}
                            }
                            ```
                            
        - **LinkedList**
            - 특징
                - **비순차적으로 데이터를 추가, 삭제**시 ArrayList보다 빠르다
            - **출력방법 (Arraylist와 같지만, 추가적인 방법)**
                - **Iterator**<String> iterator = **list.iterator();**     // iterator()메소드를 호출하여 Iterator 객체를 얻습니다.
                    
                    **while(iterator.hasNext())** {      // 현재 위치에서 다음 데이터가 있는지 확인하는 메서드. 있으면 true → 계속 메서드를 돌림
                        **String** str = **iterator.next();**      // 다음 데이터를 가져오는 메서드
                        System.out.println(str);
                    }
                    
        - **Vector**
            - 특징
                - ArrayList의 구버전 (거의 똑같음)
                - ArrayList나 LinkedList보다 성능이 떨어진다
                - **멀티쓰레드용**으로 만들어놓음
- **Stack**
    - **LIFO**
    - **Stack**<String> s = **new Stack** <String>;
    - **s.push (”자바”)**  →  add와 같은 기능
    - **s.empty()**  →  Stack이 비었는지 확인
    - **s.peek()**  → 출력 대기전인 데이터 확인하기
    - **s.pop()**  →  가장 마지막부터 출력
    - **s.size()**  →  Stack의 사이즈
- **Queue**
    - **FIFO**
    - **Queue**<String> queue = **new LinkedList**<String>();
    - **queue.offer(”토끼”)**  →  add와 같은 기능
    - **queue.isEmpty**  →  Queue가 비었는지 확인
    - **queue.peek()**  →  출력 대기전인 데이터 확인하기
    - **queue.size()**  →  Queue의 사이즈
    - **queue.poll()**  →  pop과 같은 기능. 하나씩 출력
- **Map 컬렉션**
    - **HashMap**
        - 해쉬 테이블로 사용할 수 있는 클래스
        - 해쉬 테이블이란 여러개의 통을 만들어 놓  **키값**을 이용하여 **데이터**를 넣을 통번호를 계산하는 자료구조
        - **중복이 안되고**, 마지막에 넣은 값이 저장됨
        - HashMap <**String, Integer**> hashtable = new HashMap <String, Integer> (**100**);
            - String → 키값
            - Integer → 데이터
            - 100 → 100개의 통으로 구성된 해쉬테이블
        - hashtable.**put**(”해리, new Integer(95));
            - **키값**인 해리를 통해 통번호를 **계산**해서 **키값과 데이터를 넣는다**
        - Integer num = hashtable.**get(”해리”);**
            - **키값**으로 통번호를 **계산**하고, 그 통안에서 키값과 일치하는 **데이터를 리턴**
        - hashtable.**remove**(”해리”);
            - **키값**으로 통번호를 **계산**하고, 그 통안에서 키값과 일치하는 **데이터를 삭제**
        - **출력방법**
            - System.out.println(**hm**);
            - System.out.println(hm.**keySet()**);  →  키값만 출력
            - System.out.println(hm.**values()**);   →  데이터만 출력
            - System.out.println( hm.**get( "woman" )**);   →  키값을 통해 데이터 출력
            - Set<String> keys = hm.**keySet()**;    →  키값은 for문으로 출력하고 데이터값을 따로 출력
            for (String key: keys) {
                System.out.println(key + "=" + hm.get(key));
            }
        - **최대, 최소값 구하기**
            - main
                - max_min 메서드 호출, **HashMap<String,Integer> 자료형** 적어주기
                
                ```java
                public static void main(String args[]) {
                				int data[] = new int[5];
                				Scanner sc = new Scanner(System.in);
                				System.out.println("정수 5개를 입력하세요?");
                				for(int i=0; i< data.length; i++)
                					data[i] = sc.nextInt();
                				
                				HashMap<String, Integer> result = max_min(data);
                				System.out.println("최대값 =" + result.get("최대값"));
                				System.out.println("최대값 =" + result.get("최소값"));
                			    
                			    sc.close();
                			}
                ```
                
            - max_min 메서드
                - 배열에 있는 값을 **HashMap 객체**를 생성해서 **put()**을 이용하여 저장
                - return hm → **참조값으로 리턴**하기
                
                ```java
                static HashMap<String, Integer> max_min(int data[]){
                				/*int result[]=new int[2];
                				result[0] = data[0];
                				result[1] = data[0];*/
                				int result[] = {data[0], data[0]};
                				
                				for(int i=1; i < data.length; i++){
                					if(result[0] < data[i]) result[0] = data[i]; //max	
                					if(result[1] > data[i]) result[1] = data[i]; //min
                				}
                				
                				HashMap<String, Integer> hm = new HashMap<String, Integer>();
                				hm.put ("최대값", result[0]);
                				hm.put ("최소값", result[1]);
                				return hm;		
                	}
                ```
                
    - **TreeMap**
        - HashMap과 같은데 **키값을 정렬**함
- **Set 컬렉션**
    - **HashSet**
        - 내부적으로 **HashMap을 이용해서 만들어졌으며** HashSet이란 이름은 해싱을 이용해서 구현했기 때문에 붙여진 것
        - **순서**와 **중복**이 **없음**
        - 로또 번호 만들기
            - Hashset은 **중복이 안되기** 때문에 **add()**로넣기만하면 됨
            - Iterator를 이용하여 출력 (향상된 for문도 가능)
            
            ```java
            public static void main(String[] args) {
            		HashSet<Integer> set = new HashSet<Integer>();
            		Random random = new Random();
            		
            		while (set.size() <6) {
            			set.add(random.nextInt(45)+1);
            		}
            		
            		Iterator<Integer> iterator = set.iterator();
            		while(iterator.hasNext()) {
            			System.out.println(iterator.next()+ "\t");
            		}
            	}
            ```
            
    - **TreeSet**
        - 이진 검색 트리 (binary search tree)라는 자료구조의 형태로 데이터를 저장하는 컬렉션 클래스이며, 정렬이 된다
- **ArrayCopy**
    - System.**arraycopy**(arr1, 10,
                                 arr2, 2, 3);
        - **arr1[10] ~** 의 값을 **arr2[2] ~ arr2[4]**에 부분 복사
    - char copy [] = new char **[arr1.length];**
    System.**arraycopy**(arr1, 0,
                                 copy, 0, arr1.length);
        - arr1.length 만큼 copy배열을 만들고
        - arr1[0] ~ 의 값을 copy[0] ~ copy[arr1.length]에 넣어버린다
- **Generics**
    - jdk1.5에 처음 도입
    - <>안에 들어갈 수 있는 자료형
        - Wrapper클래스
        - String
        - 사용자 정의 클래스형-참조형
    - API 형식 표기
        - Class ArrayList<E> : E의 ArrayList라고 읽는다
            - ArrayList : 원시 타입(raw type)
            - E : 원소 (Element)
            K : 키(Key)
            T : 타입(Type)
            V : 값(Value)
