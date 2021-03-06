# 슬숲슬숲
소개 : 지친 업무, 스트레스에서 벗어나 서울 도심 속에서 마음의 안정을 찾고 휴식을 취할 수 있는 공원을 쉽게 찾아볼 수 있도록 하였습니다. 서울 도심 내에도 숨은 좋은 공원들이 생각보다 많고, 공원에 대한 직관적인 정보를 지도를 통해 보다 쉽게 얻을 수 있으면 사용자들이 시간 절약을 통해 산책을 즐길 수 있을 것 같아 만들게 되었습니다.

------------

# 1. 제작기간 & 팀원 소개
- 2022/05/19~05/13 (4일간 진행)
- 팀원(4인)  
 * 홍종훈(팀장) : CSS, 서버연결(제공)
 * 최성우 :  상세페이지 구현, DB관리, 네이버 지도 기능 구현, 메인페이지 도움
 * 이담 :  메인페이지 구현, DB관리
 * 이효리 :  회원가입 로그인 기능구현, DB관리, Mongodb에 JSON파일 정보 연결

------------

# 2. 사용 기술

##### Front-end
* Jquery
* AJAX
* Bulma
* Boostrap
* Naver Map API
* HTML / CSS

##### Back-end
* Flask
* MongoDB
* Jinja2
* Pyjwt
* Python3

##### Deploy
* AWS EC2



------------

# 3. 핵심 기능

##### 회원가입 및 로그인
* 회원가입시 중복된 아이디가 있는지 확인합니다.
* JWT를 이용하여 로그인과 회원가입을 구현했습니다.


##### 네이버 지도 기능
* 네이버 지도 API를 사용하여 상세페이지에서 선택한 공원의 위치와 정보를 확인할 수 있게 구현했습니다.

##### JSON 파일 Mongodb 구축
* 서울시에서 제공해주는 공원 JSON파일을 다운받아, Mongodb에 업로드 하였습니다.

##### 서브페이지 기능
* 새로운 주소 루트를 만들어서 Mongodb에 저장되어있는 콜렉션을 호출하여서 사용하였습니다.
* Jinja2를 이용해서 /detail/<title> 경로를 render.template을 통해 필요한 정보를 넘겨서 {{}}형태로 사용하였습니다.



------------

# 4. 완성된 프로젝트
[![Video Label](https://i.ytimg.com/vi/shvw-figPT0/hqdefault.jpg?sqp=-oaymwEXCOADEI4CSFryq4qpAwkIARUAAIhCGAE=&rs=AOn4CLAIkv-LX0vIQvFZHj2s1LrT3o6VVw)](https://www.youtube.com/watch?v=shvw-figPT0)

------------

# 5. 이슈 사항 

* 서울시 공공데이터 공원현황 API를 JSON 파일로 다운받아 MongoDB에 import한 후 오류가 발생하였습니다.
  *  Get요청을 할 때, 경로를 JSON파일이 저장된 Collection으로 지정했었는데 오류가 발생해서,  Jinja2템플릿 언어를 사용하여 새로운 경로를 지정해주고 데이터를 받아와서 사용할 수 있었습니다.

* 서버에 연결후 로그인 페이지에서 회원가입은 되는데, 로그인을 시도하였을때 "Typeerror: Object of type bytes is not JSON serializable"라는 에러메세지와 함께 로그인 기능이 작동하지 않는 문제
  * Localhost환경에서는 decode가 이미 되어있어서 불필요한 코드라 생각되어 포함시키지 않았지만, AWS 연결후 배포 시 리눅스 환경에서는 .decode('utf-8')을 포함하여 문제를 해결하였습니다. (아래 코드 참고)
  ```
   token = jwt.encode(payload, SECRET_KEY, algorithm='HS256').decode('utf-8')
  ```


* 네이버 지도를 사용해서 선택한 공원을 지도에 보여주고 싶은데, 어떤 값이 필요한지 판별하는 것에 어려움이 있었습니다.
  * 공원 정보를 스크래핑을 하는 것이 아닌, 이미 저장되어 있는 데이터 중 원하는 공원의 정보만 찾아서 render.templates를 통해 상세페이지로 넘겨 주었습니다. 
    넘겨받은 정보를 Jinja2를 통해 {{정보}}를 활용해서 필요한 공원정보를 사용할 수 있었습니다.
  * 저장되어있는 위도와 경도를 바탕으로 네이버 지도에 값은 전달해야 되는데, 저장되어 있는 값이 문자열이라 geocode를 통해 숫자로 바꿔 네이버에 넘겨서 올바른 위치를 보여줬습니다.
