How to make a Server   
=
### Install CentOS 7   
Minimal Version   
Don't make root. Just make Admin User   
***
### Basic Apache Server
$sudo yum install -y httpd   
$sudo yum install -y httpd-devel   
$sudo firewall-cmd --permanent --add-service=http   
$sudo firewall-cmd --permanent --add-service=https   
$sudo firewall-cmd --reload   

$sudo systemctl enable httpd   
$sudo systemctl start httpd   

$sudo yum install -y gcc    
$sudo yum install -y lynx   
$lynx localhost   
`Testing 123.. `      

$ip addr `127.0.0.1` `192.168.0.~`   

try http://192.168.0.~/ in another computer which shared samd ip   
try http:// ~ . ~ . ~ . ~ : ~ / in any computer which not shared same ip   

$cd /var/www/html   
$sudo vi index.html `Hello` `This is test html` `Welcome~~~`   
try ip address   

$cd ~   
$systemctl stop httpd   
You can't see the website. It's error.    
***
### Python 3.x Version
$sudo yum install -y https://centos7.iuscommunity.org/ius-release.rpm   
$sudo yum -y update   
$yum search python36   
$sudo yum install -y python36u python36u-devel ~~python36u-libs~~ ~~python36u-pip~~    
- - -
### Virtual Environment
$python3 -m pip install virtualenv --user   
$mkdir venv
$cd venvs
$virtualenv flaskenv   
$source flaskenv/bin/activate   
`(flaskenv)`$   
`(flaskenv)`~~$deactivate~~   
$   
- - -
### Flask    
`(flaskenv)`$python3 -m pip install flask   
`(flaskenv)`$vi pyfile.py   
> from flask import Flask    
> 
> app = Flask(__name__)   
> 
> @app.route('/')   
> def hello_world():   
> > return 'Hello World!'   
> 
> if __name__ == '__main__':   
> > app.run()   

`(flaskenv)`$python3 hello.py `Warning. This is a development server. `    
- - -
### mod_wsgi
`(flaskenv)`$sudo yum install -y mod_wsgi    
`(flaskenv)`$vi hello.wsgi `import sys` `sys.path.insert(0, '/home/server_user/venvs')` `from hello import app as application`     
`(flaskenv)`$sudo vi /etc/httpd/conf.d/flasks.conf    

~~loadmodule 추가함~~
`<VirtualHost *:80>
  WSGIDaemonProcess hello user=nobody group=nobody threads=5
  WSGIScriptAlias / /home/server_user/venvs/hello.wsgi
  <Directory /home/server_user/venvs>
    WSGIProcessGroup hello
    WSGIApplicationGroup %{GLOBAL}
    Order allow,deny
    Allow from all
  </Directory>
</VirtualHost>`


`(flaskenv)`$sudo systemctl restart httpd `403 Forbidden`    
`(flaskenv)`$sudo tail /etc/httpd/logs/error_log    
`(flaskenv)`$sudo vi /etc/httpd/conf/httpd.conf `<Directory /> ~~Require all denied~~ -> Require all granted`    
 
`(flaskenv)`$sudo systemctl restart httpd `500 Internal Server Error`  
`(flaskenv)`$sudo tail /etc/httpd/logs/error_log `cant't read wsgifile.wsgi`    
`(flaskenv)`$chmod 704 /home/server_user   

`(flaskenv)`$sudo systemctl restart httpd `403 Forbidden`  
`(flaskenv)`$sudo tail /etc/httpd/logs/error_log `access to / denied`   
`(flaskenv)`$chmod 705 /home/server_user   

`(flaskenv)`$sudo systemctl restart httpd `500 Internal Server Error`  
`(flaskenv)`$sudo tail /etc/httpd/logs/error_log `cant't read wsgifile.wsgi`    
`(flaskenv)`$chmod 665 wsgifile.wsgi
