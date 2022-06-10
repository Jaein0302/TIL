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
            - **list.add(”포도”);**  → list에 추가
            - **list.set(1, ”포도");**  → 해당 인덱스에 있는 자료를 변경
            - **list.size();**  →  list에 있는 데이터의 수를 구하는 메서드
            - **get(index)**  →  list에 있는 자료 가져오
            - **list.remove(”키위”);**  →  같은 값이 있더라도 가장 빠른 index의 키위만 제거
            - **list.IndexOf(”사과”);**  →  앞에서부터 찾음
            - **list.lastIndexOf(”사과”);**  →  뒤에서부터 찾음
            - **list1.contains(list2.get(i));** →  list2.get(i)가 list1에 포함되어 있는지 확인
            - **list2.removeAll(list1);**  →  list2에서 list과 공통된 부분 제거
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
