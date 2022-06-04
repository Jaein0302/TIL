- **StringBuffer**는 멀티쓰레드에 안전하도록 **동기화**되어 있습니다.
    - **동기화**란 공유 데이터 사용 중에 그 공유 데이터를 다른 스레드(실행 중인 프로그램을 프로세스라고 하며 프로세스를 구성하는 세부 작업 단위)가 사용하지 못하도록 만드는 것을 의미합니다.
    - 멀티쓰레드로 작성된 프로그램이 아닌 경우 StringBuffer의 동기화는 불필요하게 성능만 떨어뜨리게 되어 StringBuffer에서 쓰레드의 동기화만 뺀 **StringBuilder**가 추가되었습니다.
    - StringBuilder는 StringBuffer와 똑같은 기능으로 작성되어 있습니다.
        - StringBuffer : 멀티쓰레드용
        - StringBuilder : 싱글쓰레드용
    - 주요 메서드
        - **sb.append(값)**
            - StringBuffer, StringBuilder 뒤에 값을 붙인다
        - **sb.insert(인덱스, 값)**
            - 특정 인덱스부터 값을 삽입한다
        - **sb.delete(인덱스, 인덱스)**
            - 특정 인덱스부터 인덱스까지 값을 삭제한다
        - **sb.indexOf(값)**
            - 값이 어느 인덱스에 들어있는지 확인한다
        - **sb.substring(인덱스, 인덱스)**
            - 인덱스부터 인덱스까지 값을 잘라온다
        - **sb.length()**
            - 길이 확인
        - **sb.replace(인덱스, 인덱스, 값)**
            - 인덱스부터 인덱스까지 값으로 변경
        - **sb.reverse()**
            - 글자 순서를 뒤집는다
- **String 클래스**
    - String str1 = "자바";
    String str2 = "자바";
        - str1 == str2  →  **같음**
    - String str1 = new String ("자바");
    String str2 = new String ("자바");
        - str1 == str2  →  **다름**
    - String str1 = "자바";
    String str2 = "자바";
        - str1.equals(str2)  →  **같음**
    - String str1 = new String ("자바");
    String str2 = new String ("자바");
        - str1.equals(str2)  →  **다름**
- **STRING 메서드**
    - scharAt( )  → 문자열을 문자로
    - substring( )  → 해당 index를 추출
    - trim( )  →  앞뒤 공백 제거
    - concat( ) → 문자열 연결
    - toUpperCase( ) → 대문자로 변환
    - toLowerCase( ) → 소문자로 변환
    - replace( ‘a’ , ‘b’)  →  a를 b로 모두 변경
    - toString( )  →  문자열로 나오게 (평소에 자동으로 생성)
    - lastIndexof( ) → 문장 마지막에서부터 해당 index 찾기
    - Indexof( )  →  문장 앞에서부터 해당 index 찾기
- **StringTokenizer**
    - **StringTokenizer** stok = new **StringTokenizer**("사과 배 복숭아"); → 객체생성
    - **stok.nextToken( );  →**  토큰에 저장되어있는 순으로 나옴
    - while(**stok.hasMoreTokens()**) {  
    System.out.println(stok.nextToken()); }   →  토큰이 있는 동안 반복
    - StringTokenizer stok = new StringTokenizer(  "사과,배|복숭아",  **",|"**  );   → 구분자를 지정할 수 있음  .
    - StringTokenizer stok = new StringTokenizer("사과=10 | 초콜렛=3 | 샴폐인=1",   **"=|"**,   **true**);   →  true를 쓰면 구분자도 전부 출력
- **Split**
    - String phone = "010-1234-5678";
    String[] s = phone.**split("-");**
    for(String token : s)
    System.out.println(token);
