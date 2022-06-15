# 0614

- **Buffered 예**
    - **FileReader**를 통해 jumsu 파일을 열고 참조변수를 **BufferedReader**의 생성자에 호출시킨다
    - **readLine()**을 통해 jumsu에 있는 문자들을 한줄씩 출력하고 String으로 저장
    - **split()**의 역할은 하나의 열(lateral)씩 분리시키는 것
    - split()으로 **String 배열에 저장**시키고 참조변수들을 이용하여 **Student2 객체 생성   → 잘 모르겠음**
    - Student2 객체를 이용하여 **ArrayList에 저장**
        - Split() 이용
        
        ```java
        static void input(ArrayList<Student2> as) {
        		BufferedReader reader = null;
        		try {
        			reader = new BufferedReader(new FileReader("src/ex18_5_test/jumsu.txt"));
        			while(true) {
        				String str = reader.readLine();
        				if (str == null)
        					break;
        				String[] sep = str.split(" ");
        				as.add(new Student2(sep[0], Integer.parseInt(sep[1]), Integer.parseInt(sep[2]), Integer.parseInt(sep[3])));
        }
        ```
        
        - StringTokenizer 이용
        
        ```java
        static void input(ArrayList<Student2> as) {
        		BufferedReader reader = null;
        		try {
        			reader = new BufferedReader(new FileReader("src/ex18_5_test/jumsu.txt"));
        			while(true) {
        				String str = reader.readLine();
        				if (str == null)
        					break;
        				StringTokenizer sep = new StringTokenizer(str);
        				as.add(new Strudent2(sep.nextToken(),
        				 						       	Interger.parseInt(sep.nextToken()),
        				 						      	Interger.parseInt(sep.nextToken()),
        				 						      	Interger.parseInt(sep.nextToken())));
        				}
        
        ```
        
- **File**
    - **File file = new file(”.”);**
        - File 객체 생성
        - **“.”** → 현재 디렉토리  / **“..”** → 상위 디렉토리
    - **File arr[] = file.listFiles();**
        - 서브 **디렉토리**와 **파일** 목록 불러오기
    - for(int cnt=0; cnt< arr.length; cnt++) {
        String name = arr[cnt].getName();
        if(arr[cnt].isFile()) 
            System.out.printf("%-25s %7d \t", name, arr[cnt].length());
        else
            System.out.printf("%-25s <DIR> \t", name);
        
        
            long time = arr[cnt]**.lastModified();**
            GregorianCalendar calendar = new GregorianCalendar();
            calendar.**setTimeInMillis(time);**
            System.out.printf (”%tF %tT \n”, calendar, calendar);
        }
        
        - 파일이름, 크기, 날짜 순으로 출력
        - **lastModified** : 파일 수정 날짜 확인 (밀리세컨드를 리턴함)
        - **setTimeInMillis  :** 밀리세컨드를 날짜형식으로 변환
        - %1$tF : YYYY-mm-dd 포맷의 날짜
        - %1$tT : HH:MM:SS 포맷
- **Directory 생성**
    - **File f = new File (”C:\\newDirectory”);**
        - 새로 생성할 폴더 주소값으로 File 객체 생성
    - **f.mkdir()**
        - 디렉토리 생성
- **파일 생성**
    - File f = new File (**”C:\\newDirectory\\newFile.txt”);**
        - 새로 생성할 파일 주소값으로 File 객체 생성
    - **f.createNewFile()**
        - 파일 생성
- **파일 삭제 & Directory 삭제**
    - **File f = new File("C:\\newDirectory\\newFile.txt”);**
        - 지울 파일 주소값으로 File 객체 생성
    - **f.delete()**
        - 파일 삭제
    - Directory 삭제도 같은 방식
- **Buffer를 이용해 파일 복사하기**
    - original파일의 BufferedInputStream 객체 생성
    - target 디렉토리 생성
    - target파일의 BufferedOutputStream 객체 생성
    - original 파일로부터 read, target 파일로 write  →  **read하면서 readBytes에 저장, 길이값 리턴 / 길이값이 -1 될때까지 readBytes를 target 파일에 저장**
        - try {
        **in = new BufferedInputStream (new FileInputStream(originaldir + "\\" + originalFileName));**
        
        		**File to = new File(targetdir);**
        
        		if(!to.exists()) {
        			if(**to.mkdir()**) {
        				System.out.println("새로 만든 디렉토리 이름:" + to.getPath());
        			} else {
        				System.out.println("디렉토리 생성에 실패했습니다");
        				return;
        			}
        		}
        
        		**out = new BufferedOutputStream (new FileOutputStream(targetdir + "\\\\" + targetFileName));**
        
        		int readCount;
        	        byte[] readBytes = new byte[1024];
        
        	    while((readCount = **in.read(readBytes)**) != -1) {
        	    	out.**write(readBytes);**  
        	    }
        
- **BufferedWriter**
    - BufferedWriter는 **버퍼 사이즈를 지정**해서 write하는 형식이다
    - 지정한 **버퍼가 다 차기 전**까지는 파일에 실제로 **데이터를 쓰지 않는다**
    - 해결책
        - **bw.flush()**
            - 남아있는 데이터도 같이 보낸다는 의미
        - **bw.close()**
            - fw.close()가 아닌 bw.close()를 해줘야함
            - 보조기반 스트림인 버퍼의 스트림을 정상적으로 닫을때 자동으로 flush() 메서드를 호출한다
    - ileWriter fw = null;
    BufferedWriter bw = null;
    try {
    fw = new FileWriter("src/ex18_7_Buffered_flush/output.txt");
    
    		bw = new BufferedWriter(fw,5);
    
    		char arr[] = {'내', '꺼', '인', '1', '듯',
    					  '2', '내', '꺼', '3', '아',
    					  '닌', '4', '내', '꺼', '5',
    					  '같', '은', '6', '너'};
    		for(int cnt=0; cnt<arr.length; cnt++)
    			bw.write(arr[cnt]);
                    **bw.flush();**
    		}catch (IOException ioe) {
    			System.out.println("파일로 출력할 수 없다");
    	}
    	finally {
    		try {
    			**bw.close();**     → bw.flush()와 둘중에 하나만 선택해서 적으면 됨
    		}
    		catch (Exception e) {
    			System.out.println(e.getMessage());
    		}
    	}
    }
    
- **Serialization**
    - **직렬화(Serialization)**
        - 객체를 네트워크를 통해 전송하거나 파일에 저장하기 위해서 **객체를 스트림으로 만드는 작업**이 필요하다
    - **GregorialCalendar 객체를 스트림으로 [직렬화]**
        - **ObjectOutputStream** 객체 생성
        - **WriteObject** 를 통해 객체를 스트림으로 만들어서 출력
        - ObjectOutputStream out = null;
        try {
            out = **new ObjectOutputStream(new FileOutputStream("ouput.dat"));**
            out.**writeObject**(new GregorianCalendar(2019,0,14));
        }
    - **GoodsStock 객체를 스트림으로 [역직렬화]**
        - **class GoodsStock implements Serializable**
            - 일반적으로 만든 객체는 직렬화할 수 있는 인터페이스로부터 구현받아야 한다
        - **ObjectInputStream** 객체 생성
        - **in.readObject**는 Object를 리턴하는데 **다운캐스팅**을 통해 GoodsStock의 객체를 생성
        - **while(true)**를 쓰는 이유는 **EOFException**이 파일로부터 더이상 읽을 객체가 없을때 예외를 잡아줌 (break 역할)
        - ObjectInputStream in = null;
        try {
            in = **new ObjectInputStream(new FileInputStream("output2.dat"));**
            while(true) {		
                **GoodsStock gs = (GoodsStock) in.readObject();**
                System.out.println(gs.toString());
            }
    - **Transient**
        - **인스턴스 필드**만 직렬화 대상이다
        - **생성자, 메서드, 정적 필드**는 직렬화 대상이 아니다
        - **transient** : 직렬화에서 제외 시킬 필드를 표시하는 키워드
            - private **transient** String passwd;
    - **SerialVersionUID**
        - **serialVersionUID** :  같은 클래스임을 알려주는 **식별자 역할**을 하며 Serializable 인터페이스를 구현한 클래스를 **컴파일하면 자동적으로 serialVersionUID 정적 필드가 추가**됩니다.
        - 클래스의 구성요소가 **하나라도 바뀌면** 완전히 **다른 serialVersionUID가 부여**된다
        - 하지만 **명시적으로 클래스에 serialVersionUID 가 선언**되어 있으면 컴파일 시 추가 되지 않기 때문에 **동일한 값을 유지**할 수 있습니다.
            - **private static final long serialVersionUID = 7514349784749178140L;**
