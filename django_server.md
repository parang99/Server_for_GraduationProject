How to make a Server   
=
### Install CentOS 7   
DVD Version   
Software: Server - Use GUI. Add development Tools.   
Make root. Make Admin User.   
Log in root.   
***   
### Basic Apache Server
#yum install -y httpd httpd-devel       
#firewall-cmd --permanent --add-service=http   
#firewall-cmd --permanent --add-service=https    
#firewall-cmd --reload   

#httpd -v `Apache/2.4.6 (CentOS)`    

#systemctl enable httpd   
#systemctl start httpd    
~~#systemctl stop httpd~~      

#ip addr `127.0.0.1` `192.168.0.12`   

try localhost in same computer     
try http://192.168.0.~/ in another computer which shared samd ip     
try http:// ~ . ~ . ~ . ~ : ~ / in any computer which not shared same ip     
  
***
### Python 3.x Version
#yum install -y https://centos7.iuscommunity.org/ius-release.rpm   
#yum -y update   
~~#yum search python36~~   
#yum install -y python36u python36u-devel ~~python36u-libs~~ ~~python36u-pip~~    
- - -
### mod_Wsgi
#yum install -y python36u-mod_wsgi   
- - -
### Install Django 
#pip3.6 install --upgrade pip    
~~#pip3.6 install django~~ 3.x.x version is installed. This make an error in basic example.    
#pip3.6 install django==2.1     
- - -
### Create Project
#cd /opt    
#django-admin startproject app1     
Admin user로 로그인 후 위의 명령어에 sudo를 붙여서 실행하고 장고를 2.1처럼 버전을 명시하지 않고 설치하면 `django-admin`이 실행 안된다. 그 때는 `chmod o+w .`을 한 후에 `django-admin`을 하면 된다. write 권한이 없어서 안 되는 것이다. 프로젝트를 만들면 다시 `chmod 755 .`로 원래 권한으로 바꿔줘라.    

opt   
|__ app1   
..........|__ manage.py   
..........|__ app1    
....................|__ __init__.py    
....................|__ settings.py    
....................|__ urls.py    
....................|__ wsgi.py     

#vi /etc/httpd/conf.d/vhost.conf   
   
    ~~<VirtualHost *:80>~~ at home
    <VirtualHost *:3000> at school
    	ServerName localhost
	#Alias /static/admin /usr/lib64/python3.6/site-packages/django/contrib/admin/static/admim
	
	WSGIScriptAlias / /opt/app1/app1/wsgi.py
	WSGIDaemonProcess app1 python-path=/opt/app1/app1
	WSGIProcessGroup app1
	
	WSGIApplicationGroup %{GLOBAL}
	
	    <Directory /opt/app1/app1>
	   	  <Files wsgi.py>
		  	AllowOverride none
			Require all granted
		    </Files>
	    </Directory>
    </VirtualHost>

위처럼 파일을 새로 만드는 대신 `#vi /etc/httpd/conf/httpd.conf`에 내용을 추가해도 무방하다. 근데 이렇게 하는게 더 편하다.    

#vi /opt/app1/app1/wsgi.py      

    import sys
    path = '/opt/app1'
    if path not in sys.path: 
      sys.path.append(path)    
      
#vi /opt/app1/app1/settings.py   
Modify `ALLOWED_HOSTS = []` -> `ALLOWED_HOSTS = ['*']`    

#vi /etc/httpd/conf/httpd.conf   
Modify `Require all denied` -> `Require all granted` in <Directory />   
Add `ServerName localhost`
~~Add `Listen 3000`~~ or modify `Listen 80` -> `Listen 3000` at school   

#systemctl restart httpd   

try localhost in same computer    
try http://192.168.0.~/ in another computer which shared samd ip     
try http:// ~ . ~ . ~ . ~ : ~ / in any computer which not shared same ip    

!!!!! `django` !!!!!    
***
### Django
#pip3 install pandas     
~~#pip3 install numpy~~     
#pip3 install sklearn     
#pip3 install djangorestframework==3.9.0     

#cd /opt/app1   
#python3 manage.py startapp predict   
#ls `app1` `manage.py` `predict`   

opt   
|__ app1   
..........|__ manage.py   
..........|__ app1    
....................|__ __init__.py    
....................|__ settings.py    
....................|__ urls.py    
....................|__ wsgi.py   
..........|__ predict    
....................|__ __init__.py    
....................|__ admin.py    
....................|__ apps.py    
....................|__ models.py   
....................|__ tests.py    
....................|__ views.py    
....................|__ migrations    
..............................|__ __init__.py    

#python3 manage.py runserver   
try 127.0.0.1:8000 in same computer

#cd /opt/app1/app1   
#vi settings.py    
INSTALLED_APPS에 `predict` `rest_framework` 추가   

#vi /opt/app1/app1/urls.py   

    import predict.views
        path('test/<str:word>/', predict.views.testfun), 
        path('cate/<str:word>/', predict.views.display), 

#vi /opt/app1/predict/views.py    

    from django.http import HttpResponse, JsonResponse
    from rest_framework.views import APIView
    from ios_predict import predict_category
    
    def testfun(request, word):
    	s = "Keyword : " + word
	return HttpResponse(s)
	
    def display(request, word):
    	s = "Keyword : " + word + ", " + "Prediction : " + predict_category(word)
	return HttpResponse(s)
	


Download ios_predict.py, ios_prediction.pkl, preprocessing_chunk0.csv   
Move above files in /opt/app1   

#python3 manage.py runserver   

try 127.0.0.1/cate2/<str>    
Ex) `127.0.0.1/cate2/나이키/` `127.0.0.1/cate2/iphone 11`   


~~학교에서 여기까지 함. 학교는 인터넷 잡을 때 수동으로 해야함. 라우팅 해제하고. ~~

#python3 manage.py makemigrations   
#python3 manage.py migrate   
~~이 코드가 왜 필요한지 모른다. ~~
 
#python3 manage.py createsuperuser   
Username: admin   
Email address: admin@example.com   
Password: * 2   
Superuser created successfully   
~~이 코드가 왜 필요한지 모른다. ~~

- - -

2. Run on other computers   
try ~ . ~ . ~ . ~ : ~ `Gateway Timeout` `The gateway did not receive a timely response from the upstream server or application`     
/etc/httpd/conf.d/vhost.conf에 `WSGIApplicationGroup %{GLOBAL}`를 추가하면 같은 공유기 내 컴퓨터에서 가능. LTE 사용해도 가능.    

#python3 manage.py runserver를 하지 않아도 된다. #systemctl start httpd만 해주면 알아서 잘 된다.    

try ~ . ~ . ~ . ~ : ~ /test/lulu `Keyword : lulu`   
try ~ . ~ . ~ . ~ : ~ /cate/lulu `FileNotFoundError at /cate/lulu/` `[Errno 2] No such file or directory: 'ios_prediction.pkl'` `[Errno 2] No such file or directory: 'preprocessing_chunk0.csv'`    

#vi /opt/app1/ios_predict.py    
Modify `ios_prediction.pkl` -> `/opt/app1/download/ios_prediction.pkl`    
Modify `preprocessing_chunk0.csv` -> `/opt/app1/download/preprocessing_chunk0.csv`   
#cd /opt/app1    
#mkdir download    
#mv ios_prediction.pkl download    
#mv preprocessing_chunk0.csv download    

try ~ . ~ . ~ . ~ : ~ /test/lulu `Keyword : lulu, Prediction : [310]`       

The End! 다음은 db연동으로 돌아옵니다~    
- - -
추가...  
#firewall-cmd --permanent --zone=public --add-port=1998/tcp   
방화벽 포트 열기  
#firewall-cmd --permanent --zone=public --add-port=3000/tcp    at school     
***
Flask로 몇주를 허비하다가 Django를 3일만에 성공해서 Django로 한다. 개인적으로 초보자의 경우 Django를 사용하는게 나아 보인다. django-admin startproject가 알아서 폴더랑 파일들을 만들어줘서 좋다. Flask는 내가 알아서 폴더, 파일 위치를 고려해야해서 어렵다.    
***
3000 포트 연결 후 ModuleNotFoundError ios_predict 에러가 났다. 
#vi /opt/app1/ios_predict.py   
Add #!/usr/bin/env python3   

PermissionError at ios_prediction.pkl   
sestatus
setenforce 0
***
Reference:     
https://ossian.tistory.com/94   
https://victorydntmd.tistory.com/258   
https://beomi.github.io/2018/03/09/Truncated_or_oversized_response_headers_received_from_daemon_process_django_wsgi/    
https://serverfault.com/questions/514242/non-responsive-apache-mod-wsgi-after-installing-scipy/514251#514251    
