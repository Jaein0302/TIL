# 회원 정보 수정

- **/memberUpdate.net**
    - **MemberUpdateAction**
        - MemberLoginProcessAction (로그인 처리)에서 로그인 성공시 **세션에 id값을 저장**했었다
        - 이 id값을 불러와서 DAO의 member_info 메서드의 매개변수로 사용하여 **id에 해당하는 값들을 select절**로 불러온다
        - **저장된 Member** 클래스를 **request 객체**에 memberinfo라는 이름으로 저장한다
        - **updateForm**으로 **dispatcher**방식으로 전송한다
    - **코드**
        
        ```java
        public class MemberUpdateAction implements Action {
        	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        		
        		HttpSession session = request.getSession();
        		String id = (String) session.getAttribute("id");		
        		
        		MemberDAO mdao = new MemberDAO();
        		Member m = mdao.member_info(id);
        		
        		ActionForward forward = new ActionForward();
        		forward.setPath("member/updateForm.jsp");
        		forward.setRedirect(false);
        		request.setAttribute("memberinfo", m);
        		return forward;
        	}
        }
        ```
        
    - **DAO - member_info**
        - **해당 id**에 대한 데이터 값을 불러와서 다시 **Member객체에 담아서 반환**한다
        - **코드**
            
            ```java
            public Member member_info(String id) {
            		Member m = null;
            		Connection con = null;
            		PreparedStatement pstmt = null;
            		ResultSet rs = null;
            		
            		try {
            			con = ds.getConnection();
            			
            			String sql = "select * from member where id=?";			
            			pstmt = con.prepareStatement(sql);
            			pstmt.setString(1, id);
            			rs = pstmt.executeQuery();
            			
            			if (rs.next()) { 
            				m = new Member();
            				m.setId(rs.getString(1));
            				m.setPassword(rs.getString(2));
            				m.setName(rs.getString(3));
            				m.setAge(rs.getInt(4));
            				m.setGender(rs.getString(5));
            				m.setEmail(rs.getString(6));
            				m.setMemberfile(rs.getString(7));
            			}
            		} catch (Exception se) {
            			se.printStackTrace();
            		} finally {
            			try {
            				if (rs != null)
            					rs.close();
            			} catch (SQLException e) {
            				System.out.println(e.getMessage());
            			}
            			try {
            				if (pstmt != null)
            					pstmt.close();
            			} catch (SQLException e) {
            				System.out.println(e.getMessage());
            			}
            			try {
            				if (con != null)
            					con.close();
            			} catch (SQLException e) {
            				System.out.println(e.getMessage());
            			}
            		}
            		return m;
            ```
            
    - **updateForm**
        - el기법을 사용하여 memberinfo에 저장된 property들을 불러와서 화면에 표시한다
            - **${memberinfo.id}**
        - 성별은 남과 여중 check를 해야하는 동적 요소이기 때문에 script를 이용
            - ${memberinfo.gender}의 값이 **gender의 value값**이기 때문에 value의 값에 따라 checked가 바뀌는 것을 설정
            - **$("input[value=" + gender + "]").prop("checked",true);**
        - 이메일은 keyup을 통해 바로 오류를 표시할 수 있게 설정
            - **이메일의 value값**과 **pattern**과 **비교**해서 해당 내용을 보여줄 수 있게 해준다
            - **checkemail**의 역할은 **false**이면 나중에 submit일때 페이지가 넘어가지 않게 해주는 용도이다
        - form에서 submit할때 **나이가 숫자 아닐경우**, **checkmail이 false**일 경우 페이지가 넘어가지 않게 한다
        - 프로필 사진이 파일 이름 옆에 나오게 한다
            - **<c:if test='${empty memberinfo.memberfile}'>**
                - **empty의 의미**는 해당하는 값이 **빈값**이면 if == null 과 같은 의미이다
            - **<c:set var='src' value='${"memberupload/"}${memberinfo.memberfile}'/>**
                - **변수를 저장하는 기능인데 잘 모르겠다**
            - **업로드는 다시 공부**
        - **코드**
            
            ```java
            <%@ page language="java" contentType="text/html; charset=UTF-8"%>
            <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
            
            <html>
            <head>
            	<link href="css/join.css" type="text/css" rel="stylesheet">	
            	<style>
            	h3 {
            		text-align: center; color: #1a92b9;
            	}
            	input[type=file]{
            		display:none;
            	}
            	</style>
            </head>
            <body>
            	<jsp:include page="../board/header.jsp"/>
            	<form name="joinform" action="updateProcess.net" method="post" enctype="multipart/form-data">
            		<h3>회원 정보 수정</h3>
            		<hr>
            		<b>아이디</b>
            		<input type="text" name="id" value="${memberinfo.id}" readonly>
            		
            		<b>비밀번호</b>
            		<input type="password" name="pass" value="${memberinfo.password}" readonly>
            	
            		<b>이름</b>
            		<input type="text" name="name" value="${memberinfo.name}" placeholder="Enter name" required>
            		
            		<b>나이</b>
            		<input type="text" name="age" value="${memberinfo.age}" maxlength="2" placeholder="Enter age" required>
            		
            		<b>성별</b>
            		<div>
            			<input type="radio" name="gender" value="남"><span>남자</span>
            			<input type="radio" name="gender" value="여"><span>여자</span>
            		</div>
            		
            		<b>이메일 주소</b>
            			<input type="text" name="email" value="${memberinfo.email}" placeholder="Enter email" required>
            		<span id="email_message"></span>
            		
            		<b>프로필 사진</b>
            		<label>
            			<img src="image/attach.png" width="10px">
            			<span id="filename">${memberinfo.memberfile}</span>
            			<span id="showImage">
            				<c:if test='${empty memberinfo.memberfile}'>
            					<c:set var='src' value='image/profile.png'/>
            				</c:if>
            				<c:if test='${! empty memberinfo.memberfile}'>
            					<c:set var='src' value='${"memberupload/"}${memberinfo.memberfile}'/>
            				</c:if>
            				<img src="${src}" width="20px" alt="profile">							
            			</span>
            			<%-- accept: 업로드할 파일 타입을 설정ㅎ랍니다.
            				 <input type="file" accept="파일 확장자|audio/*|video/*|image/*">
            				 1) 파일 확장자는 .png, .jpg, .pdf, .hwp 처럼 (.)으로 시작되는 파일 확장자를 의미합니다.
            				 	예)accept=".png, .jpg, .pdf, .hwp"
            				 2) audio/* : 모든 타입의 오디오 파일
            				 3) image/* : 모든 타입의 이미지 파일
            		    --%>
            		    <input type="file" name="memberfile" accept="image/*">		
            		</label>	
            		<div class="clearfix">
            			<button type="submit" class="submitbtn">수정</button>
            			<button type="button" class="cancelbtn">취소</button>
            		</div>
            	</form>
            <script>
            	var gender = '${memberinfo.gender}';
            	$("input[value=" + gender + "]").prop("checked",true);  // 성별 체크
            
            	$(".cancelbtn").click(function(){
            		history.back();
            	});
            	
            	//처음 화면 로드시 보여줄 이메일은 이미 체크 완료된 것이므로 기본 checkemail=true이다.
            	var checkemail=true;
            	$("input:eq(6)").on('keyup',
            			function() {
            				$("#email_message").empty();
            				//[A-Za-z0-9_]와 동일한 것이 \w
            				//+는 1회이상 반복을 의미한다. {1,}와 동일하다
            				//\w+는 [A-Za-z0-9_]를 1개이상 사용하라는 의미인다
            				var pattern = /^\w+@\w+[.]\w{3}$/;
            				var email = $("input:eq(6)").val();
            				if (!pattern.test(email)) {
            					$("#email_message").css('color', 'red').html("이메일형식이 맞지 않습니다.");
            					checkemail = false;
            				} else {
            					$("#email_message").css('color', 'green').html("이메일형식이 맞습니다.");
            					checkemail = true;					
            				}
            		});
            	
            	$('form').submit(function(){
            		if(!$.isNumeric($("input[name='age']").val())) {
            			alert("나이는 숫자를 입력하세요");
            			$("input[name='age']").val('').focus();
            			return false;
            		}
            		
            		if(!checkemail) {
            			alert("email 형식을 확인하세요");
            			$("input:eq(6)").focus();
            			return false;
            		}
            	})
            	
            	$('input[type=file]').change(function(event){
            		console.log($(this).val());  // C:\fakepath\Hydrangeas.jpg
            		var inputfile = $(this).val().split('\\');
            		var filename = inputfile[inputfile.length - 1];
            		
            		var pattern = /(gif|jpg|jpeg|png)$/i // i(ignore case)는 대소문자 무시를 의미
            		
            		if(pattern.test(filename)) {
            			$('#filename').text(filename); //inputfile.length - 1 = 2
            			
            			var reader = new FileReader();  // 파일을 읽기 위한 객체 생성
            			//DataURL 형식(접두사 data:가 붙은 URL이며 바이너리 파일을 Base64로 인코딩하여 ASCII 문자열 형식으로 변환한것)으로 파일을 읽어온다
            			//(참고-Base64 인코딩은 바이너리 데이터를 Text로 변경하는 인코딩이다.)
            			//네트워크탭에서 실행 후 Headers 확인해보자
            			
            			//읽어온 결과는 reader객체의 result 속성에 저장된다
            			//event.target.files[0] : 선택한 그림의 파일객체에서 첫번째 객체를 가져온다
            			reader.readAsDataURL(event.target.files[0]);
            			
            			reader.onload = function() { //읽기에 성공했을때 실행되는 이벤트 핸들러
            			//$('#showImage').html('<img width="20px" src="' + this.result + '">');
            			$('#showImage > img').attr('src', this.result);
            			};
            		} else {
            			alert('이미지 파일(gif,jpg,jpeg,png)이 아닌 경우는 무시된다)');
            			$('#filename').text('');
            			$('#showImage > img').attr('src', 'image/profile.png');
            			$(this).val('');
            		}
            	})		
            
            </script>
            </html>
            ```
            
            -
