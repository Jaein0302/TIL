# 0628 background, boxmodel, span

- **background**
    - **background-image** → 배경이미지 속성
        - url() 함수를 이용해서 파일명 설정
    - **background-repeat** →  이미지 반복
        - **repeat-x**  → x축으로 반복
        - **repeat-y** → y축으로 반복
        - **no-repeat** → 반복 안함
        - **repeat** → 반복: 디폴트
    - **사용 예시**
        - #third { **background-image**: url(image/flower.jpg);
                     **background-repeat**: no-repeat;
                     **background-position**: center bottom}
    - **background-attachment** → 스크롤 여부를 결정
        - background-attachment : **scroll;**
            - 스크롤 내리면 배경 이미지가 올라간다
        - background-attachment : **fixed;**
            - 스크롤 내려도 배경 이미지는 고정
- **boxmodel - div**
    - **box-sizing : content-box**
        - CSS 표준에 의해 정의된 **기본 스타일**
        - **width와 height** 속성은 **오로지 콘텐츠만**을 **포함 (padding, border, margin**을 **포함하지 않는다)**
        - 기본적으로 padding을 주면 배경색이 padding까지 영향을 미침
        - margin은 border 바깥 영역으로서 배경색이 영향을 주지 못함
        - div {**box-sizing: content-box;**}
        #third {**background: yellow**; width:100px; height:100px;
        **padding**: 10px 5px;}
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/20928b58-603e-478a-8cb4-303963f2b445/Untitled.png)
            
    - **background-clip: content-box**
        - background의 색을 어디까지 칠할지 결정해줌
        - background-clip을 content-box로 지정해주면 padding을 지정해도 content만 배경색이 바뀜
        - #third_clip {
        **background: yellow;** width: 100px; height: 100px; **padding**: 20px;
        **background-clip: content-box;**
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/10b5e0c6-d97d-4a28-820e-41a0c02764dc/Untitled.png)
            
    - **padding**
        - padding: 10px;
            - **위, 아래, 오른쪽, 왼쪽** 모두 10px
        - padding : 10px 5px;
            - **위,아래**는 10px가 적용되고, **오른쪽, 왼쪽**은 5px
        - padding : 10px 7px 8px;
            - **위** 10px, **오른쪽, 왼쪽**은 7px, **아래**는 8px
    - **box-sizing : border-box**
        - **width와 height** 속성이 **padding 및 border를 포함** (**margin을 포함하지 않는다**)
- **span**
    - 기본적으로 **inline 스타일**로 **width, height 크기 조절이 불가능**하고, **줄바꿈이 되지 않는다**
    - inline 속성은 새 라인에서 시작하지 못하고 옆에 다른 요소 배치 가능하다
    - 다른 속성을 보여주고 싶을때는 **display** 속성을 이용한다
    - **display 속성**
        - display : **block** (블록 박스)
        - display : **inline** (인라인 박스)
        - display : **inline-block** (block와 inline의 중간 형태)
            - **block** 속성인 **width와 height 크기 조절 가능**하고 **padding, border, margin 조절 가능**
            - **inline** 속성인 새 라인에서 시작하지 못하고 **옆에 다른 요소 배치**
        - display : **none**
            - 화면에 보이지 않게 만든다
            - **div:hover** + **span** {
                  **display: block;   →  커서를 갖다대었을때 보일 수 있게 한다**
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/87040119-c2bc-454e-8850-b0cf3ddee0f9/Untitled.png)
            
    - block 속성에서 **세로 정렬을** 하고 싶을때는 height 만큼 수치를 주면 된다
        - **line-height: 100px;**
- **list-style**
    - **ol** → ordered list (번호가 생김)
    - **ul** → unordered list  (목록 기호가 생김)
    - **list-style-type: none**  →  번호나 목록기호가 안생김
- **border-collapse**
    - border-collapse: **collapse**  →  경계선을 하나로 합침
    - border-collapse: **seperate**→  경계선을 분리함
- **caption-side : bottom**
    - 표의 제목 위치로 표 아래에 표시되도록 한다
- **empty-cells**
    - **hide**  →  빈 셀의 테두리를 그리지 않는다
    - **empty-cells** : show  →  기본값 - 빈 셀의 테두리를 그린다
