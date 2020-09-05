How to make a Server   
=
### Install CentOS 7   
Minimal Version   
Don't make root. Just make Admin User   
***
$sudo yum install -y httpd   
$sudo firewall-cmd --permanent --add-service=http   
$sudo firewall-cmd --permanent --add-service=https   
$sudo firewall-cmd --reload   

$systemctl enable httpd   
$systemctl start httpd   

$sudo yum install lynx   
$lynx localhost   
`Testing 123.. `   
~~$sudo yum install -y elinks~~ no operating   

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
Reference:   
https://wikidocs.net/16275
