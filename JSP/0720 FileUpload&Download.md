- **파일 업로드 & 다운로드 페이지 만들기**
    - **파일 업로드 환경설정**
        - [www.servlets.com](http://www.servlets.com) → COS FIle Upload Library → cos-22.05.zip ekdnsfhem
        - WebContent/WEB-INF/lib에 **cos.jar** 복사 붙여넣기
        - WebContent 폴더 아래 **upload 폴더** 생성하기 (나중에 배포하고 나서 이쪽으로 저장이 될것이기 때문이다)
    - **FileUploadForm**
        - 파일을 **업로드**하는 페이지
            - **method="post”**
                - 메서드는 post방식
            - **enctype="multipart/form-data”**
                - 파일을 업로드하기 위해서 필요한 인코딩 타입
            - **input type="file”**
                - 파일 형태를 업로드 할 수 있음
        - **전체코드**
            
            ```java
            <%@ page language="java" contentType="text/html; charset=EUC-KR"%>
            <html>
            <head>
            <meta name="viewport" content="width=device-width, initial-scale=1">
            <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
            
            <style>
            	.container{margin-top: 3em; width: 500px}
            	.input-group-text{width: 74px;}
            </style>
            </head>
            <body>
            	<div class="container">
            		<%-- 파일을 업로드하기 위해서 enctype 속성을 "multipart/form-data"로 설정한다 --%>
            		<form action="fileUpload.jsp" method="post" enctype="multipart/form-data">
            			<p class="h4 mb-4 text-center">파일 업로드 폼</p>
            			<div class="input-group mb-3">
            				<div class="input-group-prepend">
            					<span class="input-group-text mb-1">작성자</span>
            				</div>
            				<input type="text" class="form-control" name="name">
            			</div>
            			
            			<div class="input-group mb-3">
            				<div class="input-group-prepend">
            					<span class="input-group-text">제  목</span>
            				</div>
            				<input type="text" class="form-control" name="subject">
            			</div>
            			
            			<div class="form-group">
            				<input type="file" class="form-control-file border" name="fileName1">
            			</div>
            			
            			<div class="form-group">
            				<input type="file" class="form-control-file border" name="fileName2">
            			</div>
            			
            			<button type="submit" class="btn btn-info my-4 btn-block">Submit</button>
            		</form>	
            	</div>
            </body>
            </html>
            ```
            
    - **fileUpload**
        - 업로드한 파일의 **속성값들을 불러오는** 페이지
        - 불러온 속성값들을 post 방식으로 **fileCheck.jsp** 파일로 전달한다
            - **<%@ page import="com.oreilly.servlet.MultipartRequest"%>**
                - 업로드 기능이 있는 **MultipartRequest** 객체를 import
            - **<%@ page import="com.oreilly.servlet.multipart.DefaultFileRenamePolicy" %>**
                - 파일 이름 **중복 처리**를 하기 위해 DefaultFileRenamePolicy 객체를 import
            - **<%@ page trimDirectiveWhitespaces="true" %>**
                - true일 경우 공백을 없애주는 기능
                - **servlet과 jsp 둘다 출력스트림을 이미 쓰고 있는데 2번 출력되는것을 막기위해 쓰임**
            - **String uploadPath = application.getRealPath("upload");**
                - C:\java_국비\workspace\JSP\src\main\**webapp**\upload
                    - webapp의 하위 폴더에 생성된 upload 폴더
                    - 하지만 실제로는 이 폴더에 업로드 되지 않는다
                - C:\java_국비\workspace\.**metadata**\.plugins\org.eclipse.wst.server.core\tmp0\wtpwebapps\JSP\upload
                    - upload폴더는 metadata 폴더에도 생성되는데, **metadata** 폴더는 **이클립스**에서 작성하고 있는 프로젝트가 **톰켓에서 실행**될 때 배포되는 **폴더**이다
                    - 따라서, 업로드 기능도 이클립스에서 작성되고 있기 때문에 이 폴더에 업로드된다.
            - **MultipartRequest** multi = new MultipartRequest(**request**,
                                                                                         **uploadPath**,
                                                                                         **size**,
                                                                                         **"euc-kr"**,
                                                                                         **new DefaultFileRenamePolicy()**);
                - MultipartRequest 객체는 업로드를 담당한다
                - 첫 번째 인자 **request** →폼에서 가져온 값을 얻기 위해 request객체를 전달해 준다
                - 두 번째 인자 **uploadPath** → 업로드될 파일의 위치이다
                - 세 번째 인자 **size** → 업로드 할 크기이며 지정 크기보다 크면 Exception이 발생한
                - 네 번째 인자 **"euc-kr"** → 파일 이름이 한글인 경우 처리하는 부분이다
                - 다섯 번째 인자 → 똑같은 파일을 업로드 할 경우 **중복되지 않도록** 자동으로 파일이름을 변환해주는 기능을 한다
            - **name=multi.getParameter("name");**
                - MultipartRequest 객체에 저장된 name의 값을 불러온다
            - **systemName1 = multi.getFilesystemName("fileName1");**
                - 시스템상의 파일명을 불러온다
            - **origfileName1 = multi.getOriginalFileName("fileName1");**
                - 업로드한 원본 파일명을 불러온다
        - **전체코드**
        
        ```java
        <%@ page language="java" contentType="text/html; charset=EUC-KR"%>
        <%@ page import="com.oreilly.servlet.MultipartRequest"%>
        <%@ page import="com.oreilly.servlet.multipart.DefaultFileRenamePolicy" %>
        <%@ page trimDirectiveWhitespaces="true" %>
        
        <% 
        	String uploadPath = application.getRealPath("upload");
        	out.print("upload 경로 : " + uploadPath);
        	
        	int size = 10*1024*1024; // 파일 최대 크기를 10MB로 지정한다
        	String name = ""; String subject = ""; String systemName1 = "";
        	String systemName2 = ""; String origfileName1 = ""; String origfileName2 = "";
        	
        	try{
        		MultipartRequest multi = new MultipartRequest(request,
        									uploadPath,
        									size,
        									"euc-kr",
        								new DefaultFileRenamePolicy());
        								
        		name=multi.getParameter("name");
        		subject=multi.getParameter("subject");
        		
        		//name=fileName1의 input에서 올린 파일의 시스템상의 파일명을 얻어온다
        		//중복된 파일명의 경우 파일명 맨뒤에 숫자를 붙여 처리한다
        		systemName1 = multi.getFilesystemName("fileName1");
        		
        		//name=fileName1의 input에서 업로드한 원본 파일명을 얻어 온다
        		origfileName1 = multi.getOriginalFileName("fileName1");
        		
        		systemName2 = multi.getFilesystemName("fileName2");		
        		origfileName2 = multi.getOriginalFileName("fileName2");
        			
        	} catch(Exception e) {
        		e.printStackTrace();
        		out.print("에러입니다.");
        	}
        %>
        <html>
        	<head>
        		<title>파일 업로드</title>
        	</head>
        <body>
        	<form name="filecheck" action="fileCheck.jsp" method="post">
        		<input type = "hidden" name="name" value="<%=name %>">
        		<input type = "hidden" name="subject" value="<%=subject %>">
        		
        		<input type = "hidden" name="systemName1" value="<%=systemName1 %>">
        		<input type = "hidden" name="systemName2" value="<%=systemName2 %>">
        		
        		<input type = "hidden" name="origfileName1" value="<%=origfileName1 %>">
        		<input type = "hidden" name="origfileName2" value="<%=origfileName2 %>">
        		<input type = "hidden" name="uploadPath" value="<%=uploadPath %>">
        		<input type = "submit" value="업로드 확인 및 다운로드 페이지 이동">	
        	</form>
        </body>
        </html>
        ```
        
    - **fileCheck**
        - 저장된 속성값들 **화면에 나타내기**
            - **request.setCharacterEncoding("euc-kr");**
                - post방식이기 때문에 한글화시켜서 요청을 받는다
            - getParameter를 통해 속성값들을 받아오고, 화면단에 뿌려준다
        - **다운로드**를 ****위한 **링크** 만들어놓기
            - <a href="file_down.jsp?**file_name=**<%=**systemName1** %>"><%=origfileName1 %></a>
                - **file_name**의 값을 **system주소값**으로 **저장**시켜 링크를 만든 것이다
        - **전체코드**
            
            ```java
            <%@ page language="java" contentType="text/html; charset=EUC-KR"%>
            <html>
            <head>
            <style>
            	table {text-align : center; margin : 0 auto; border : solid black 1px; border-collapse: collapse;}
            	td {border : solid black 1px; border-collapse: collapse;}}
            </style>
            </head>
            <body>
            <%
            	request.setCharacterEncoding("euc-kr");
            	String name = request.getParameter("name");
            	String subject = request.getParameter("subject");
            	String systemName1 = request.getParameter("systemName1");
            	String systemName2 = request.getParameter("systemName2");
            	String origfileName1 = request.getParameter("origfileName1");
            	String origfileName2 = request.getParameter("origfileName2");
            	String uploadPath = request.getParameter("uploadPath");
            %>
            <div class="container">
            	<h4>파일 다운로드 폼</h4>
            	<table class="table table-bordered table-striped">
            		<tr>
            			<td>작성자 </td><td><%=name %></td>
            		</tr>
            		<tr>
            			<td>제목 </td><td><%=subject %></td>
            		</tr>
            		<tr>
            			<td>파일명1 </td>
            			<td>
            			<a href="file_down.jsp?file_name=<%=systemName1 %>"><%=origfileName1 %></a>
            			</td>
            		</tr>
            		<tr>
            			<td>파일명2 </td>
            			<td>
            			<a href="file_down.jsp?file_name=<%=systemName2 %>"><%=origfileName2 %></a>
            			</td>
            		</tr>
            		<tr>
            			<td>uploadPath </td><td><%=uploadPath %></td>
            		</tr>
            	</table>
            </div>
            </body>
            </html>
            ```
            
    - **filedown**
        - ServletContext context = **pageContext.getServletContext();**
        String sDownloadPath = **context.getRealPath(savePath);**
            - **pageContext.getServletContext();**  →  pageContext의 기본 객체를 반환
            - **context.getRealPath(savePath);**  ****→  webapp으로부터 savePath까지의 경로 (여기서 savePath는 upload로 선언했음)
        - String sDownloadPath = **application.getRealPath(savePath);**
            - 위 두문장을 합치면 이렇게 쓸 수 있다
        - String sMimeType = **context.getMimeType(sFilePath);**
            - SFilePath의 Mime Type을 구할 수 있다
            - 여기서는 **image/jpeg**가 결과로 나온다
            - **Content-Type 종류**
                - **image/png** → 브라우저는 해당 컨텐츠를 이미지로써 간조하게 됩니다.
                - **application/octet-stream** → 미확인 Binary 정보를 의미하며, 이경우 브라우저는 파일을 다운로드하는 형태로 동작합니다.
                - **text/javascript** → 브라우저는 Content를 javascript문서로 취급하게 됩니다.
        - if (sMimeType == null)
            sMimeType = "**application/octet-stream"**;
            - **application/octet-stream**은 미확인 Binary 정보를 의미하며, 여기서도 타입이 null인 경우에 이것으로 대체한다.
        - String sEncoding = **new String(fileName.getBytes("euc-kr"), "ISO-8859-1");**
            - 해당 **바이트 배열(fileName.getBytes("euc-kr"))**을 주어진 **캐릭터 셋(ISO-8859-1)**으로 간주하여 스트링을 만드는 생성자
        - response.setHeader(**"Content-Disposition", "attachment**; filename=" + sEncoding);
            - **Content-Disposition**의 값을 **"attachment; filename=" + sEncoding**으로 설정하겠다
            - **attach**일 경우 다운로드, **inline**일 경우에는 즉시 출력한다는 의미이다
        - BufferedInputStream in = new **BufferedInputStream(new FileInputStream(sFilePath));**
            - sFilePath로 지정한 파일에 대한 입력 스트림을 생성
        - BufferedOutputStream out2 = new **BufferedOutputStream(response.getOutputStream());**
            - 웹 브라우저로의 출력 스트림 생성
        - while ((numRead = **in.read(b, 0, b.length))** != -1) { //읽을 데이터가 존재하는 경우
             out2.write(b, 0, b.length);
        }
            - 바이트 배열 b의 0번부터 b.length 크기 만큼 읽어온다
        - **전체코드**
            
            ```java
            <%--ServletContext을 이용한 예제 --%>
            <%@ page contentType="text/html; charset=EUC-KR"%>
            <%@ page import="java.io.*"%> 
            <%@ page trimDirectiveWhitespaces="true" %>  
            
            	String fileName = request.getParameter("file_name");
            	System.out.println("fileName = " + fileName);
            	
            	String savePath = "upload";
            	
            	//서블릿의 실행 환경 정보를 담고 있는 객체를 리턴합니다.
            	//(application 내장 객체를 리턴합니다)
            	ServletContext context = pageContext.getServletContext();
            	String sDownloadPath = context.getRealPath(savePath);
            	//위 두 문장을 한 문장으로 나타내면 다음과 같습니다.
            	//String sDownloadPath = application.getRealPath(savePath);
            	
            	// "\" 추가하기 위해 "\\" 사용합니다.
            	String sFilePath = sDownloadPath + "\\" + fileName;
            	System.out.println(sFilePath);
            	
            	byte b[] = new byte[4096];
            	
            	//sFilePath에 있는 파일의 MimeType을 구해옵니다.
            	String sMimeType = context.getMimeType(sFilePath);
            	System.out.println("sMimeType>>>" + sMimeType);
            	
            	/* 
            		1. Content-Type : 전송되는 Content가 어떤 유형인지 알려주는 목적을 가지고 있습니다.
            			- text/html, image/png, application/octet-stream 등의 값을 가집니다.
            		2. Content-Type을 통해서 브라우저는 해당 데이터를 어떻게 처리해야 할지 판단할 수 있게 됩니다.
            		(예)
            			1) image/png : 브라우저는 해당 컨텐츠를 이미지로써 간조하게 됩니다.
            			2) application/octet-stream : 미확인 Binary 정보를 의미하며, 이경우 브라우저는 파일을 다운로드하는 형태로 동작합니다.
            			3) text/javascript : 브라우저는 Content를 javascript문서로 취급하게 됩니다.
            	*/
            	
            	/* 
            		octet-stream은 8비트로 된 데이터를 의미하며 지정되지 않은 파일 형식을 의미합니다.
            	*/
            	if (sMimeType == null)
            		sMimeType = "application/octet-stream";
            		
            	response.setContentType(sMimeType);
            	
            	/*
            		- 이 부분이 한글 파일명이 깨지는 것을 방지해 줍니다.
            		- getBytes(캐릭터셋) : 자바 내부에 관리되는 유니코드 문자열을 인자로 지정된 캐릭터셋의 바이트 배열로 반환하는 메서드입니다.
            		- String(byte[] bytes, Charset charset)
            		  String(바이트배열, 캐릭터셋) 생성자 : 해당 바이트 배열을 주어진 캐릭터 셋으로 간주하여 스트링을 만드는 생성자입니다.
              	*/
              	
              	String sEncoding = new String(fileName.getBytes("euc-kr"), "ISO-8859-1");
            		System.out.println(sEncoding);
            		
            	/* Content-Disposition : Content가 어떻게 처리되어야 하는지 나타냅니다.
            		1) Content-Disposition:
            		   inline : 브라우저가 Content를 즉시 출력해야 함을 나타냅니다.
            		   			이미지인 경우 브라우저 내에서 즉시 출력하며, 문서인 경우 텍스트로 출력합니다.
            		2) Content-Disposition:
            		   attachment : 브라우저는 해당 Content를 처리하지 않고 다운로드하게 됩니다.
            	*/
            	
            	response.setHeader("Content-Disposition", "attachment; filename=" + sEncoding);
            	/* response.setHeader("Content-Disposition", "inline; filename=" + sEncoding); */
            	
            	/*
            	1. try-with-resource문으로 try()괄호 안에 선언된 자원은
            	   try문이 끝날 때 자동으로 close()메서드를 호출합니다.
            	   
            	2. try-with-resource문에 의해 자동으로 객체의 close()가 호출될 수 있으려면 클래스가
            	   AutoCloseable이라는 인터페이스를 구현한 것이어야 한다.
            	   형식) try() {
            		     .....
            	   		}catch(){
            	   			
            	   		}finally{}
            	*/
            	
            	try(
            			//웹 브라우저로의 출력 스트림 생성합니다.
            			BufferedOutputStream out2 = new BufferedOutputStream(response.getOutputStream());
            			
            			//sFilePath로 지정한 파일에 대한 입력 스트림을 생선합니다.
            			BufferedInputStream in = new BufferedInputStream(new FileInputStream(sFilePath));
            	   )
            	   {
            			int numRead;
            			
            			//read(b, 0 , b.length) : 바이트 배열 b의 0번부터 b.length 크기 만큼 읽어온다
            			while ((numRead = in.read(b, 0, b.length)) != -1) { //읽을 데이터가 존재하는 경우
            				out2.write(b, 0, b.length);
            			}
            		} catch (Exception e) {
            			e.printStackTrace();
            		}
            %>
            ```
