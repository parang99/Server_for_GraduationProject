# Part of the Server for Graduation Project / Graduation Project에 사용된 서버의 일부

Role of the server in graduation project   
: 안드로이드 앱에서 사용자가 입력한 sentence를 서버로 받아와 model에 input으로 사용한다. 서버는 도출된 output을 다시 안드로이드 앱으로 전송한다.    

다양한 이유로 서버의 전체 내용은 올리지 않았다.   
전체 서버와 가장 비슷한 파일은 django_server.md다.   
upload된 파일들은 graduation project에서 필요한 서버를 만들기 위해 노력한 내용들이다. 

---
CentOS7에서 Apache Server를 만들었다.   
제작한 Machine Learning Model을 사용하기 위해 Flask와 Django를 해봤다. 처음에 Flask로 시도했으나 실패하여, Django로 갈아탔다.   
서버의 DB를 사용하려고 SQLite에 도전한 부분도 있다. 

---
파일 설명
- base_server.md : CentOS7 minimal version에서 Apache Server 설치하고 테스트
- base_server_upgrade.md : 위 파일에 python, virtual environment, flask, mod_wsgi 설치
- flask_server_fail.md : 위 파일에 Apache와 mod_wsgi 연동을 시도. 결과는 실패. 
- flask_server_fail_v2.md : 위 파일과 내용은 같으나 다르게 시도. 결과는 실패. 
- django_server.md : 위 과정들로 Flask는 실패하여 Django로 도전. CentOS7 DVD version을 사용. predict는 machine learning model과 관련된 부분이다. 결과는 성공.   
- db.md : sqlite 연동 시도. Django admin page css 지원 안 됨 해결. 
- connect.md : Django Serializer. 직렬화. 미완성. 
- posting.md : form. 미완성. 


내용 중간에 나타나는 `~ . ~ . ~ . ~ : ~` 은 ip주소다. 

---
Flask와 Django를 시도하면서 느낀점 :   
* 만약 둘 중에 무엇을 이용해야 하는지 고민하는 사람이라면 이렇게 말해주고 싶다. 당신이 정확한 path에 폴더와 파일을 잘 만들수 있다면 Flask를 해라. Flask는 하나의 빈 폴더가 만들어지고 그 공간을 내가 직접 폴더와 파일을 생성하여 만들어간다.   
* 만약 당신이 위와 같이 하지 못한다면 Django를 해라. Django는 하나의 프로젝트를 생성하면 가장 기초로 필요한 폴더와 파일을 알아서 생성해주어 초보자에게 편리하다.   
* 처음에 Flask로 시도했을 때 헷갈리고 어려운 점이 많아 여러 실패를 겪었다. Django로 넘어갔을 때는 Flask보다 쉬웠고 성공했다. 그래서 나는 Flask보다 Django를 선호한다.   
