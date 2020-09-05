# Part of the Server for Graduation Project
Graduation Project에 사용된 서버의 일부
=
Role of the server in graduation project   
: 안드로이드 앱에서 사용자가 입력한 sentence를 서버로 받아와 model에 input으로 사용한다. 서버는 도출된 output을 다시 안드로이드 앱으로 전송한다.    

다양한 이유로 서버 구축의 전체 내용은 올리지 않았다.   
upload된 파일들은 graduation project에서 필요한 서버를 만들기 위해 노력한 내용들이다. 


CentOS7에서 Apache Server를 만들었다.   
제작한 Machine Learning Model을 사용하기 위해 Flask와 Django를 해봤다. 처음에 Flask로 시도했으나 실패하여, Django로 갈아탔다.   
서버의 DB를 사용하려고 SQLite에 도전한 부분도 있다.   

---
파일 설명
- base_server.md : CentOS7 minimal version에서 Apache Server 설치하고 테스트
- base_server_upgrade.md : 위 파일에 python, virtual environment, flask, mod_wsgi 설치
- flask_server_fail.md : 위 파일에 Apache와 mod_wsgi 연동을 시도. 결과는 실패다. 
- flask_server_fail_v2.md : 위 파일과 내용은 같으나 다르게 시도. 결과는 실패다. 
