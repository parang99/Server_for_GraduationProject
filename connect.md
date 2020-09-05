connect.md

#cd /opt/app1   
#python3 manage.py startapp api    

#vi /opt/app1/app1/settings.py   
Add ~~`rest_framework`~~ in INSTALLED_APPS -> already done    
Add

    REST_FRAMEWORK = {
	    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination', 
	    'PAGE_SIZE': 10
    }

#vi /opt/app1/app1/urls.py   

    from rest_framework import routers
    from api import views
    router = routers.DefaultRouter()
    router.register(r'users', views.UserViewSet)
    router.register(r'groups', views.GroupViewSet)
    router.register(r'temps', views.TempViewSet)
    urlpatterns = [
    	path('', include(router.urls)), 
	    path('api-auth/', include('rest_framework.urls',  namespace='rest_framework')),   
    ]

#vi /opt/app1/api/views.py    
 
    from django.contrib.auth.models import User, Group
    from rest_framework import viewsets
    from rest_framework import permissions
    from .serializers import UserSerializer, GroupSerializer, TempSerializer
    from data.models import Temp
    
    class UserViewSet(viewsets.ModelViewSet):
    	queryset = User.objects.all().order_by('-date_joined')
    	serializer_class = UserSerializer
    # permission_classes = [permissions.IsAuthenticated]

    class GroupViewSet(viewsets.ModelViewSet):
    	queryset = Group.objects.all()
    	serializer_class = GroupSerializer
    	permission_classes = [permissions.IsAuthenticated]

    class TempViewSet(viewsets.ModelViewSet):
    	queryset = Temp.objects.all()
    	serializer_class = TempSerializer
    #	permission_classes = [permissions.IsAuthenticated]

#vi /opt/app1/api/serializers.py    

    from django.contrib.auth.models import User, Group
    from rest_framework import serializers
    from data.models import Temp

    class UserSerializer(serializers.HyperlinkedModelSerializer):
    	class Meta:
    		model = User
	    	fields = ['url', 'username', 'email', 'groups']

    class GroupSerializer(serializers.HyperlinkedModelSerializer):
    	class Meta:
    		model = Group
    		fields = ['url', 'name']

    class TempSerializer(serializers.HyperlinkedModelSerializer):
    	class Meta:
    		model = Temp
	    	fields = ['t_id', 't_name', 't_description']
		
At school

	from django.contrib.auth.models import User, Group
    	from rest_framework import serializers
    	from data.models import Temp

    class UserSerializer(serializers.HyperlinkedModelSerializer):
    	class Meta:
    		model = User
	    	fields = ('url', 'username', 'email', 'groups')

    class GroupSerializer(serializers.HyperlinkedModelSerializer):
    	class Meta:
    		model = Group
    		fields = (url', 'name')

    class TempSerializer(serializers.HyperlinkedModelSerializer):
    	class Meta:
    		model = Temp
	    	fields = ('t_id', 't_name', 't_description')




