posting.md    

#vi /opt/app1/data/forms.py    

    from django import forms

    class TableForm(forms.form):
    	name = forms.CharField(label='name', max_length=20)
     	price = form.IntegerField(label='price')
     	etc = forms.CharField(label='etc', max_length=30)
      

#vi /opt/app1/app1/urls.py    
      
      import data.views
      ...
    	  path('table/post/', data.views.postfunction), 
        
#mkdir /opt/app1/data/templates     
#mkdir /opt/app1/data/templates/data     
#vi /opt/app1/data/templates/data/form.html     

	<form action="~~{% url 'data:postfunction' %}~~" method="post">
		{% csrf_token %}
		{{ form|linebreaks }}
		<input type="submit" value="submit" name="submit">
	</form>
	

#vi /opt/app1/data/views.py    

	from .models import Table
	from django.http import HttpResponse, JsonResponse
	from .forms import TableForm

	def postfunction(request):
		if request.method == "POST":
			form = TableForm(request.POST)
			if form.is_valid():
				obj = Table(name=form.data['name'], price=form.data['price'], etc=form.data['etc'])
				obj.save()
				return HttpResponse('success')
			return HttpResponse('fail')
		elif request.method == "GET":
			form = TableForm()
			return render(request, 'data/form.html', {'form':form})
		else:
			pass
		
***
### Android Post
