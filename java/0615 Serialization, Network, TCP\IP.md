# 0615

- **Serialization 예제**
    - **ObjectOutputStream**
        - jumsu.txt으로부터 **파일을 받아 문자열로 출력한다**
        - 문자열을 **split**으로 쪼개고, 각 쪼개진 부분은 **Student2 객체의 생성자 호출**에 이용된다
        - Student2 객체를 output파일로 보내기 위해서는 **객체를 스트림으로** 바꿔줘야 한다
            - BufferedReader reader = null;
            ObjectOutputStream out = null;
            try {
            reader = **new BufferedReader(new FileReader**("src/ex19_06_test/jumsu.txt"));
            out = new **ObjectOutputStream**(new FileOutputStream("src/ex19_06_test/output.dat"));
                
                while(true) {
                	String str = **reader.readLine();**
                	if(str == null)
                		break;
                	String[] sep = str.**split(" ");**
                	**out.writeObject(new Student2(sep[0], Integer.parseInt(sep[1]), Integer.parseInt(sep[2]), Integer.parseInt(sep[3])));**
                }
                
    - **ObjectInputStream**
        - output파일에 **스트림**으로 저장된 데이터를 가져오기 위해서는 **역직렬화**를 이용
        - **Object 형식**으로 불러와 **다운캐스팅**하여 **객체 저장**
            - ObjectInputStream in = null;
            try {
                in = **new ObjectInputStream(new FileInputStream** ("src/ex19_06_test/output.dat"));
                while(true) {		
                    Student2 s = **(Student2) in.readObject();**   
                    System.out.println(s.toString());
                }
    - Student2
        - Student2 클래스는 직렬화가능하게 **인터페이스로부터 불러와야한다**
            - public class Student2 **implements Serializable**
- **네트워크**
    - TCP/IP 프로토콜
        - 인터넷에서 사용되는 프로토콜 (통신 규칙)
    - IP 주소
        - 인터넷에 연결된 컴퓨터들을 유일하게 식별하는 것으로 통신 프로그램이 대상 컴퓨터를 찾을때 IP주소를 이용한다
        - ipconfig : ip 주소를 찾을때 사용
    - 포트
        - 통신 프로그램이 실질적으로 데이터를 교환하는 대상은 컴퓨터가 아니라 프로그램
        - 상대 컴퓨터에서 작동 중인 **여러 프로그램 중 하나를 식별**할 수 있어야 한다
        - 그런 **식별은 포트를 통해 가능**
        - 포트 : 네트워크를 통해 데이터를 주고 받을때 사용되는 **가상의 출입구**
    - TCP/IP 통신 프로그램
        - 한 프로그램은 연결을 요청하고 다른 프로그램이 그 요청을 받아서 연결을 맺음
            - 클라이언트 프로그램 : 연결을 요청하는 통신 프로그램
            - 서버 프로그램 : 연결 요청을 기다리는 통신 프로그램
        - 자바에서는 포트를 사용하기 위해 소켓을 이용한다
            - 클라이언트 소켓 : **클라이언트**, **서버 프로그램**에서 모두 사용
            - 서버 소켓 : **서버 프로그램**에서만 사용. 연결요청이 오면 **또 다른 소켓을 생성**
- **TCP/IP 통신 예**
    - **Server**
        - **서버 소켓** 생성하고 서버소켓에 연결 요청이 들어오면 **새로운 소켓**을 생성
        - **입력 스트림 객체**와 **출력 스트림 객체**를 생성
        - 데이터를 **바이트 형식**으로 수신  (데이터를 바이트로만 주고 받을 수 있음)
        - **String**을 **바이트**로 변환해서 데이터 송신
        - **String**으로 변환하여 출력
            - ServerSocket serverSocket = null;
            Socket socket = null;
            try {
                serverSocket = **new ServerSocket(9000);** 
                socket = **serverSocket.accept();**
            
                **InputStream** in = **socket**.**getInputStream();**
                **OutputStream** out = **socket.getOutputStream();**
            
                byte arr[] = new byte[128];
                in.read(arr); 
                System.out.println(new String(arr));
            
                String str = "Hello, Client";
                out.write(str.getBytes());
            }
    - **Client**
        - Socket 생성자를 호출하여 서버 프로그램으로 연결 요청이 전송된다
        - **입력 스트림 객체**와 **출력 스트림 객체**를 생성
        - **String**을 **바이트**로 변환해서 데이터 송신
        - 데이터를 **바이트 형식**으로 수신
            - Socket socket = null;
            try {
                socket = **new Socket("127.0.0.1",9000);**
            
                **InputStream** in = **socket**.**getInputStream**();
                **OutputStream** out = **socket**.**getOutputStream**();
            
                String str = "Hello, Server";
                out.**write(str.getBytes());**
            
                byte arr[] = new byte[128];
                **in.read(arr);** 
                System.out.println(**new String(arr)**); 
            }
- **TCP/IP 통신 예2**
    - 계속해서 **String** ↔ **byte** 간의 형식 변환이 필요한 문제가 생김
    - 이것을 해결하기 위해서 **InputStreamReader**와 **PrintWriter**를 사용하면 됨
    - **InputStreamReader** : Byte → String
    - **PrintWriter** : String → Byte
    - PrintWriter 클래스의 **println()**메서드로 문자열을 출력하면 **버퍼가 다 찰때까지** 모아졌다가 한번에 송신된다
    - **flush()**는 println() 메서드를 호출한 다음에 바로 문자열이 송신되도록 해준다
        - try {
            serverSocket = new ServerSocket(9002);
            socket = serverSocket.accept();
        
            **BufferedReader** reader = **new BufferedReader(new InputStreamReader**(**socket.getInputStream()**));
            **PrintWriter** writer = **new PrintWriter**(**socket.getOutputStream()**);
        
            String str = **reader.readLine();**
            System.out.println(str);
        
            **writer.println("Hello,Client2");**
            **writer.flush();**
- **TCP/IP 통신 예3 (실시간 1대1 채팅방)**
    - **ReceiverThread**
        - **수신**하는 기능
        - Socket이 필드로 사용할 수 있도록 **생성자 호출**로 넘겨준다
        - **소켓이 끊어질때까지** 계속해서 **reader.readLine**으로 화면에 출력한다
            - BufferedReader reader = null;
            try {
                reader = new BufferedReader(new InputStreamReader(socket.getInputStream()));
                **while (!socket.isClosed())** {
                    String str = reader.readLine();
            
                    if(str == null)
            	    break;
            	
                    System.out.println("수신>" + str);
            }
    - **SenderThread**
        - **키보드로 입력**받은 내용을 **송신**하는 기능
        - **bye**라고 했을때 **채팅방 끝나게** 설정 (소켓이 끊어지게)
            - PrintWriter writer=null;
            BufferedReader reader=null;
            try {
                reader = **new BufferedReader(new InputStreamReader([System.in](http://system.in/))**); 
                writer = **new PrintWriter(socket.getOutputStream()**);  
            
                    while (!socket.isClosed()) {
                        String str = **reader.readLine();**
            
                        if (str.equals("bye"))
                           break; 
            
                        **writer.println(str);**
                        **writer.flush();**
            }
    - **Server, Client**
        - **서버소켓만 close**한다 (소켓은 close하면 안됨)
        - thread1, thread2 실행
- **TCP/IP 통신 예3 (단톡방)**
    - **Server**
        - 위에서 만든 Thread를 이용해서는 서버가 여러사람들에게 **송신을 할 수가 없다**
        - 각 **Client가 연결요청이 들어올때마다** 새로운 소켓을 만들고 이 **소켓을 perClientThread의 필드로 저장**시킨다
            - ServerSocket serverSocket = null;
            try {
                serverSocket = new ServerSocket(9004);
                while (true) {
                Socket socket = serverSocket.accept();
            
                Thread thread = **new PerClientThread(socket);**
                thread.start();
            }
    - **PerClientThread**
        - **PrintWriter 리스트**
            - **생성자로 받은 소켓**을 이용하여 PrintWriter **객체를 list에 하나씩 넣는다**
            - list는 다른 실행에 간섭을 받으면 안되기 때문에 **동기화된 ArrayList로** 만들어준다
                - private static **List<PrintWriter>** list = **Collections.synchronizedList**(**new ArrayList<PrintWriter>**());
                - PerClientThread (Socket socket) {
                    **this.socket = socket;**
                    try {
                        writer = **new PrintWriter**(socket.getOutputStream());
                        **list.add(writer);** 
                }
        - **수신과 모두에게 송신**
            - 수신하는건 ReceiverThread와 크게 다르지 않다
            - 다만 **처음에 Client에서 보내준** **name**을 받아서 누가 들어왔는지 **모두에게 송신**한다
            - 그리고나서 **각 Client로부터 받은 데이터**들을 **송신**한다
                - String name = null;
                BufferedReader reader = null;
                try {
                    reader = new BufferedReader(new InputStreamReader(socket.getInputStream()));
                    **name** = **reader.readLine();**
                    **sendAll**("#" + name + "님이 들어오셨습니다");
                    
                    while (!socket.isClosed()) {
                        String str = reader.readLine();
                
                        if (str == null)
                            break;
                        **sendAll**(name + ">" + str);
                }
            - **Client가 나갔을때**는 소켓이 끊어졌을때이므로 **finally**에 나간것을 모두에게 송신한다
                - finally {
                    **sendAll**("#" + name + "님이 나가셨습니다");
    - **Client**
        - Client는 예제2에서 했던것과 크게 다르지 않다
        - **args[0]**에서 입력받은 name이 **SenderThread의 필드**로 저장이 되어 처음 **서버로 송신**하게 될 것이다 (서버는 나중에 이것을 받아 sendAll)
            - Thread thread1 = new SenderThread(socket, **args[0]**);
