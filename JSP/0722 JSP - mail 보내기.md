- **Naver smtp를 이용해서 자바로 메일 보내기**
    - **SMTP이란?**
        - Simple Mail Transfer Protocol
        - 메일을 전송할 수 있도록 해주는 프로토콜을 의미하며 네이버에서 제공
        - [mvnrepository.com](http://mvnrepository.com/)에서 JavaMail API 다운로드해서 **WEB-INF/lib 폴더**에 저장
        - 네이버 설정탭에서 **POP3/SMTP** 사용
    - **mailForm.jsp**
        - 자바 메일 API를 사용하기 위해 관련 **패키지**를 import
            - **<%@ page import="java.util.Properties" %>**  →  서버 정보를 properties 객체에 저장
            - **<%@ page import="javax.mail.*" %>***
            - ***<%@ page import="javax.mail.internet.*" %>**
        - **SMTP 서버 주소**를 지정
            - **String server = "[smtp.naver.com](http://smtp.naver.com/)";**
            - **int port=587;**
        - 서버 정보를 **Properties 객체**에 저장
            - **properties properties = new Properties();**
            - **properties.put("mail.smtp.host", server);**
            - **properties.put("mail.smtp.port", port);**
        - **TLS**(전송 계층 보안-Transport Layer Security)을 이용한 암호화 프로토콜 (데이터의 보안을 유지)
            - **properties.put("mail.smtp.ssl.protocols","TLSv1.2");**
        - 저장한 Properties객체의 값으로 **세션의 인스턴스를 생성**한다
        - Session클래스는 메일 서버를 이용하는데 필요한 **이메일 계정 정보를 관리**한다
            - **Session s = Session.getInstance(properties);**
        - **보내는 메일과 받는 주소**를 설정
            - **Address sender_address=new InternetAddress(sender);**
            - **Address receiver_address=new InternetAddress(receiver);**
        - 메일을 보내기 위한 정보를 입력하기 위해 **Message객체**를 생성
            - **Message message = new MimeMessage(s);**
        - 메일의 내용이 깨지 않도록 **content-type** 지정
            - **message.setHeader("content-type", "text/html;charset=euc-kr");**
        - **보내는 사람, 받는 사람 메일 주소 정보**를 설정
            - **message.setFrom(sender_address);**  →  보내는 메일 주소 정보
            - **message.addRecipient([Message.RecipientType.TO](http://message.recipienttype.to/), receiver_address);**  →  받는 메일 주소 정보
        - **제목, 내용, 보내는 날짜** 정보를 설정
            - **message.setSubject(subject);**
            - **message.setContent(content, "text/html; charser=euc-kr");**
            - **message.setSentDate(new java.util.Date());**
        - **지정된 프로토콜(smtp)**을 구현하는 **Transport 객체**를 만든다 (메일을 전송하는 클래스)
            - **Transport transport = s.getTransport("smtp");**
        - 인자값이 **연결할 호스트, 아이디, 비밀번호** 순으로 입력되어 연결된다
            - **transport.connect(server, MyInfo.naverid, MyInfo.naverpass);**
        - **getAllRecipients()**를 통해 메시지의 모든 수신자 주소를 가져와서 지정한 주소 목록으로 메시지를 보낸다
            - **transport.sendMessage(message, message.getAllRecipients());**
- 전체코드

```java
<%-- 보내는 사람은 smtp 서버 주소를 설정한 네이버의 "아이디@naver.com"으로 합니다 --%>
<%@ page import="ex7_java_mail.MyInfo" %>
<%@ page language="java" contentType="text/html; charset=EUC-KR" %>
<%@ page import="java.util.Properties" %>
    
<%-- 자바 메일 API를 사용하기 위해 관련 패키지를 import를 합니다 --%>
<%@ page import="javax.mail.*" %>
<%@ page import="javax.mail.internet.*" %>

<%
	request.setCharacterEncoding("euc-kr");
	String sender = request.getParameter("sender");
	String receiver = request.getParameter("receiver");
	String subject = request.getParameter("subject");
	String content = request.getParameter("content");
	
	/* JavaMail을 사용하여 이메일을 전송하기 위한 3가지 절차는 다음과 같습니다.
		1. 호스트이름, 사용자명, 비밀번호 등과 같은 호스트의 모든 정보를 저장하는 Session객체를 얻습니다.
		2. 메시지를 구성합니다.
		3. 메시지를 보냅니다
	*/
	//SMTP 서버 주소를 지정합니다.
	String server = "smtp.naver.com";
	int port=587;
	try{
		//서버 정보를 Properties 객체에 저장합니다.
		Properties properties = new Properties();
		
		//SMTP 서버 정보 설정
		//네이버일 경우 smtp.naver.com
		properties.put("mail.smtp.host", server);
		properties.put("mail.smtp.port", port);
		
		//SSL(Secure Sockets Layer)과 같은 TLS(전송 계층 보안-Transport Layer Security)는
		//네트워크를 통해 전송될때 데이터의 보안을 유지하기 위한 암호화 프로토콜입니다.
		//메일 전송에 사용되는 프로토콜을 TLSv1.2로 지정합니다.
		properties.put("mail.smtp.ssl.protocols","TLSv1.2");
		
		//저장한 Properties객체의 값으로 세션의 인스턴스를 생성합니다.
		//public static Session getInstance(Properties props)
		//Session클래스는 사용자의 이름, 암호, 메일 서버와 같이
		//메일 서버를 이용하는데 필요한 이메일 계정 정보를 관리합니다.
		Session s = Session.getInstance(properties);
		
		//보내는 메일과 받는 주소를 설정합니다.
		Address sender_address=new InternetAddress(sender);
		Address receiver_address=new InternetAddress(receiver);
		
		//메일을 보내기 위한 정보를 입력하기 위해 Message객체를 생성합니다.
		Message message = new MimeMessage(s);
		
		//보내는 메일의 내용이 한글일 경우 깨지지 않도록 content-type을 지정합니다.
		message.setHeader("content-type", "text/html;charset=euc-kr");
		
		//보내는 메일 주소 정보를 설정합니다.
		message.setFrom(sender_address);
		
		//받는 메일 주소 정보를 설정합니다.
		//Message.RecipientType는 Message클래스에서 허용하는 수신자 유형을 정의합니다.
		//Message.RecipientType.TO : 받는 사람을 의미합니다.
		message.addRecipient(Message.RecipientType.TO, receiver_address);
		
		//제목 정보를 설정합니다.
		message.setSubject(subject);
		
		//내용의 정보를 설정합니다.
		message.setContent(content, "text/html; charser=euc-kr");
		
		//보내는 날짜를 설정합니다.
		message.setSentDate(new java.util.Date());
		
		//public Transport getTransport(String protocol) throws NoSuchProviderException
		//지정된 프로토콜(smtp)을 구현하는 Transport 객체를 가져옵니다.
		//메일을 전송하는클래스입니다.
		Transport transport = s.getTransport("smtp");
		
		//첫번째 인자는 연결할 호스트입니다.
		//두번째 인자는 아이디, 세번째 인자는 비밀번호를 의미합니다.
		transport.connect(server, MyInfo.naverid, MyInfo.naverpass);
		
		//getAllRecipients():메시지의 모든 수신자 주소를 가져옵니다.
		//지정한 주소 목록으로 메시지를 보냅니다.
		transport.sendMessage(message, message.getAllRecipients());
		
		//연결을 종료합니다.
		transport.close();
		
		out.println("<h3>메일이 정상적으로 전송되었습니다.</h3>");
	}catch(Exception e) {
		out.println("SMTP 서버가 잘못 설정되었거나, 서비스에 문제가 있습니다");
		e.printStackTrace();
	}
%>
```
