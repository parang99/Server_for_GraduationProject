How to make a Server   
=
### Install CentOS 7   
Minimal Version   
Don't make root. Just make Admin User   
***
### Apache
$sudo yum install -y httpd   
$sudo yum install -y httpd-devel   
~~$httpd -v~~ `2.4.6`   
~~$which apxs~~ `/bin/apxs`   
~~$whereis apxs~~ `/usr/bin/apxs`   
- - -
### Python 3.x Version
$sudo yum install -y https://centos7.iuscommunity.org/ius-release.rpm   
$sudo yum update   
$yum search python36   
$sudo yum install -y python36u   
$sudo yum install -y python36u-devel   
~~$sudo yum install -y python36u-libs~~   
~~$sudo yum install -y python36u-pip~~   

 $cd /usr/bin   
 ~~$ls -al | grep python~~   
 $sudo rm -i /usr/bin/python   
 ~~$ls -al | grep python~~   
 $sudo ln -s /usr/bin/python3.6 /usr/bin/python   
 ~~$ls -al | grep python~~
 ~~$python --version~~ `Python 3.6.8`   
- - -
### Virtual Environment
$python -m venv test   
$source test/bin/activate   
`(test)`$   
`(test)`~~$deactivate~~   
$   
- - -
### Flask 
`(test)`$python -m pip install flask   
~~$pip install --upgrade pip~~   
***
### Modify yum
To use wget, you have to `yum install wget` but it doesn't work because of python3 installation.   
Yum is operated in python2. We change /usr/bin/python from python2 to python3.   
~~$cat /usr/bin/yum~~ `#!/usr/bin/python`   
$sudo vi /usr/bin/yum `python` -> `python2`   
~~$cat /usr/bin/yum~~ `#!/usr/bin/python2`  

$sudo vi /usr/libexec/urlgrabber-ext-down `python` -> `python2`   

$sudo yum install wget
***
### mod_wsgi
`(test)`$wget https://codeload.github.com/GrahamDumpleton/mod_wsgi/tar.gz/4.6.8   
Source code from here: https://github.com/GrahamDumpleton/mod_wsgi/releases   
My source code from here: https://modwsgi.readthedocs.io/en/latest/release-notes/version-4.6.8.html   

`(test)`~~$ls~~ `4.6.8` `test`   
`(test)`$tar zxvf 4.6.8   
`(test)`~~$ls~~ `4.6.8` `mod_wsgi-4.6.8` `test`   
`(test)`$cd mod_wsgi*   
`(test)`~~$whereis apxs~~ `/usr/bin/apxs`   
`(test)`~~$whereis python~~ `/usr/bin/python`   
`(test)`$./configure --with-apxs=/usr/bin/apxs   

$sudo yum install gcc   

`(test)`$make   
`(test)`~~$make install~~ ` ... install: cannot create regular file. apxs:Error: Command failed with rc=65536`   
`(test)`$sudo make install ` ... chmod 755 /usr/lib64/httpd/modules/mod_wsgi.so`   
- - -
### Connect Apache and mod_wsgi
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
$sudo yum install elinks   
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
Reference:    
https://blog.chemdev.net/14   
https://pentestlab.tistory.com/73?category=738399   
