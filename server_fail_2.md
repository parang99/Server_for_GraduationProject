How to make a Server   
=
### Install CentOS 7   
Minimal Version   
Don't make root. Just make Admin User   
***
### Python 3.x Version
$sudo yum install -y https://centos7.iuscommunity.org/ius-release.rpm   
$sudo yum -y update   
$yum search python36   
$sudo yum install -y python36u   
$sudo yum install -y python36u-devel   
~~$sudo yum install -y python36u-libs~~   
~~$sudo yum install -y python36u-pip~~   

$sudo yum install -y wget   
$sudo yum install -y gcc   

 ~~$cd /usr/bin   
 $ls -al | grep python   
 $sudo rm -i /usr/bin/python   
 $ls -al | grep python   
 $sudo ln -s /usr/bin/python3.6 /usr/bin/python   
 $ls -al | grep python
 $python --version `Python 3.6.8`~~
- - -
### Virtual Environment
$python3 -m venv flaskenv   
$source flaskenv/bin/activate   
`(flaskenv)`$   
`(flaskenv)`~~$deactivate~~   
$   
- - -
### Apache
`(flaskenv)`$wget http://archive.apache.org/dist/httpd/httpd-2.2.29.tar.gz   
`(flaskenv)`$tar zxvf httpd-2.2.29.tar.gz   
`(flaskenv)`$cd httpd-2.2.29   
`(flaskenv)`$./configure --prefix=/home/server_user/apps/apache   
`(flaskenv)`$make   
`(flaskenv)`$make install   

~~`(flaskenv)`$sudo yum install -y httpd
`(flaskenv)`$sudo yum install -y httpd-devel~~   
~~`(flaskenv)`$httpd -v~~ `2.4.6`   
~~`(flaskenv)`$which apxs~~ `/bin/apxs`   
~~`(flaskenv)`$whereis apxs~~ `/usr/bin/apxs`   
- - -
### Flask 
`(flaskenv)`$cd ~   
`(flaskenv)`$python3 -m pip install flask   
~~$pip install --upgrade pip~~ because of upgrade 권유      
***
### Modify yum
~~To use wget, you have to `yum install wget` but it doesn't work because of python3 installation.   
Yum is operated in python2. We change /usr/bin/python from python2 to python3.   
$cat /usr/bin/yum `#!/usr/bin/python`   
$sudo vi /usr/bin/yum `python` -> `python2`   
$cat /usr/bin/yum `#!/usr/bin/python2`  
$sudo vi /usr/libexec/urlgrabber-ext-down `python` -> `python2`~~   
***
### mod_wsgi
`(flaskenv)`$wget https://codeload.github.com/GrahamDumpleton/mod_wsgi/tar.gz/4.6.8   
Source code from here: https://github.com/GrahamDumpleton/mod_wsgi/releases   
My source code from here: https://modwsgi.readthedocs.io/en/latest/release-notes/version-4.6.8.html   

`(flaskenv)`~~$ls~~ `4.6.8` `test`   
`(flaskenv)`$tar zxvf 4.6.8   
`(flaskenv)`~~$ls~~ `4.6.8` `mod_wsgi-4.6.8` `test`   
`(flaskenv)`$cd mod_wsgi*   
`(flaskenv)`~~$whereis apxs~~ `/usr/bin/apxs`   
`(flaskenv)`~~$whereis python~~ `/usr/bin/python`   
`(flaskenv)`$./configure --with-apxs=/home/server_user/apps/apache/bin/apxs --with-python=/usr/bin/python3.6   
`(flaskenv)`$make   
`(flaskenv)`$make install ` ... chmod 755 /home/server_user/apps/apache/modules/mod_wsgi.so`   
- - -
### Connect Apache and mod_wsgi
httpd.conf's path = /home/server_user/apps/apache/conf/httpd.conf   
`(flaskenv)`$cd ~   
`(flaskenv)`$cd apps/apache/conf   
`(flaskenv)`$vi httpd.conf   
add `LoadModule wsgi_module modules/mod_wsgi.so`   `User nobody`   `Group nobody`   

`(flaskenv)`$cd ~
`(flaskenv)`$mkdir python_app
`(flaskenv)`$cd python_app
`(flaskenv)`$vi hello_word.wsgi
add `import sys` `sys.path.insert(0, '/home/server_user/python_app')` `from hello_world import app as application`    




`(test)`$cd /var/www/   
`(test)`~~$ls~~ `cgi-bin` `html`   
`(test)`~~$mkdir flasktest~~ `Permission denied`   
`(test)`$sudo mkdir flasktest   
`(test)`~~$ls~~ `cgi-bin` `flasktest` `html`   
`(test)`$cd flasktest   
`(test)`$sudo mkdir flaskapp   

`(test)`$sudo vi flaskapp.wsgi   

    #!/usr/bin/python
    import sys
    sys.path.insert(0,"/var/www/flasktest/")
    from flaskapp import app as applicaiton

`(test)`$sudo vi /etc/httpd/conf/httpd.conf   

    <VirtualHost *:80>
    
        DocumentRoot /var/www/flasktest/
        
        LoadFile /usr/lib64/libpython3.6m.so.1.0
        LoadModule wsgi_module modules/mod_wsgi.so
        WSGIScriptAlias / /var/www/flasktest/flaskapp.wsgi
        WSGIScriptReloading On
        
        <Directory /var/www/flasktest/flaskapp/>
            Order deny,allow
            Allow from all 
        </Directory>
        
        <Directory /var/www/flasktest/flaskapp/static/>
            Order deny,allow 
            Allow from all 
        </Directory>
 
    </VirtualHost>   

`(test)`$cd /var/www/flasktest/flaskapp   
`(test)`$sudo mkdir templates static   
`(test)`$sudo vi _ _init_ _.py   
    
    from flask import Flask
    app = Flask(__name__)
    
    @app.route("/")
    def hello_world():
        return "Hello World"
        
    if __name__ == '__main__':
        app.run()

- - -
### Install elinks
$sudo yum install -y elinks   
- - - 
### Apache Operate
$service httpd start   
~~$service httpd stop~~   
$elinks 127.0.0.1 `Internal Server Error`   
$sudo cat /etc/httpd/logs/error_log `httpd: Could not reliably determine the server's fully qualified domain name, using localhost. localdomain. Set the 'ServerName' directive globally to suppress this message`   
$sudo vi /etc/httpd/conf/httpd.conf   
add `ServerName localhost`   
$service httpd restart   

$elinks 127.0.0.1 `Internal Server Error`   
$sudo cat /etc/httpd/logs/error_log    
$halt -p   





- - -
root https://taetaetae.github.io/2018/06/29/simple-web-server-flask-apache/















