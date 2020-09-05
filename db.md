### Practice
#cd /opt/app1    

#python3 manage.py migrate    
#ls `db.sqlite3` `manage.py` `predict` `app1` `download` `ios_predict.py` `__pycache__`    
#sqlite3 db.sqlite3    
sqlite>    
sqlite> .tables   
`auth_group` `auth_group_permissions` `auth_permission` `auth_user` `auth_user_groups` `auth_user_user_permissions` `auth_group_permissions` `django_admin_log` `django_content_type` `django_migrations` `django_session`    
sqlite> .exit   
#   
- - -
### Start
#python3 manage.py startapp data   
#ls `data` `db.sqlite3` `manage.py` `predict` `app1` `download` `ios_predict.py` `__pycache__`    
#cd data   
#ls `__init__.py` `admin.py` `apps.py` `migrations` `models.py` `tests.py` `views.py`   
migrations는 폴더. model 1개 == table 1개   
#vi models.py    
Add     

	class Category(models.Model):
		cate_id = models.IntegerField(default=0)
		cate_name = models.CharField(max_length=30)
		cate_short_name = models.CharField(max_length=10)
		cate_parent_id = models.IntegerField(default=0)
		
		def __str__(self):
			return self.cate_name

	class Product(models.Model):
		prod_id = models.IntegerField(default=0)
		prod_uid = models.IntegerField(default=0)
		prod_name = models.CharField(max_length=100)
		prod_price = models.IntegerField(default=0)
		prod_keyword1 = models.CharField(max_length=50)
		prod_keyword2 = models.CharField(max_length=50)
		prod_keyword3 = models.CharField(max_length=50)
		prod_description = models.CharField(max_length=1000)
		prod_cate_id = models.ForeignKey(Category, on_delete=models.SET_NULL, null=True)
		
		def __str__(self):
			return self.prod_name

#cd /opt/app1/app1   
#vi settings.py   
Add `'data', ` in INSTALLED_APPS   

#cd /opt/app1    
#python3 manage.py makemigrations data   
Migrations for 'data':    
   data/migrations/0001_initial.py   
model의 변화사항을 migration 폴더에 python파일로 저장하기. 아직 table 생성 안 됨.    
#python3 manage.py migrate   

#sqlite3 db.sqlite3    
sqlite>    
sqlite> .tables   
`auth_group` `auth_group_permissions` `auth_permission` `auth_user` `auth_user_groups` `auth_user_user_permissions` `auth_group_permissions` `data_category` `data_product` `django_admin_log` `django_content_type` `django_migrations` `django_session`    
sqlite> 
sqlite> .exit
***
### Admin Page
127.0.0.1/admin    
Id: admin or ap   
Pw: 관리자 or apapapap   

Operational Error: Attempt to write a readonly database   
#cd /opt/app1   
#chown apache .   
#chown apache db.sqlite3   
그래도 error가 나면   
#sestatus   
#setenforce 0   
current mode가 enforcing -> permissive로 바뀜   
해결   
***
### 

upload x   
import yes   

#vi /opt/app1/data/admin.py     

	from .models import Category, Product
	
	admin.site.register(Category)
	admin.site.register(Product)

이제 관리자페이지에서 Data로 Category와 Product를 볼 수 있다.    
***
### 
#vi /opt/app1/data/admin.py     

	from import_export.admin import ImportExportModelAdmin
	
	@admin.register(Category, Product)
	class ViewAdmin(ImportExportModelAdmin):
		pass
		
#vi /opt/app1/app1/settings.py   
Add `'import_export', ` in INSTALLED_APPS   

#pip3.6 install django-import-export    

***
### How to support CSS in my server?
지금까지 css 지원이 안돼서 관리자 페이지가 허접했었다. 드디어 이유와 해결방법을 찾음.    
#vi /etc/httpd/conf.d/vhost.conf     

	Alias /static/ /opt/app1/static/
	
	<Directory /opt/app1/static>
		Require all granted
	</Directory>
추가하기    

#mkdir /opt/app1/static   

#vi /opt/app1/app1/settings.py   

	STATIC_ROOT = os.path.join(BASE_DIR, 'static')
마지막 줄에 추가     

#cd /opt/app1    
#python3 manage.py collectstatic    

#systemctl restart httpd   
성공!    

If OperationError, try `#setenforce 0`    

***
Reference :    
https://freeprog.tistory.com/45   
https://dev-yakuza.github.io/ko/django/admin/     
