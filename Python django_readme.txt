1) Create Project
    (in terminal) django-admin startproject Myproject

2)Run the server
    C:\Python Training>cd Myproject
    python manage.py runserver
    in tab copy the url of django page and add admin (ie., http://127.0.0.1:8000/admin)

3) Create admin credentials
    python manage.py createsuperuser

4) Create schema(makemigrations) and execute schema on db(migrate)
    python manage.py makemigrations
    python manage.py migrate
    #tables created in db browser sqlite3
    then again run:
    python manage.py createsuperuser

5) Create app
    python manage.py startapp home (a new folder home(app) is getting created in Myproject folder)
    i) open settings.py from Myproject -> INSTALLED_APPS -> add 'home'
    ii) Myproject/urls.py [open urls.py from Myproject(from that it will go to various apps)]
        urls_patterns = [path('admin/', admin.site.urls), path('', include(home.urls)] and in urls.py also change the
        import stmt to from django.urls import path, include
                                         import home.urls
    iii) copy urls.py in home
    iv) home/urls.py
        from . import views
        urls_pattern = [path('',views.homepage)]

        # for about
        path('', views.homepage), path('about/', views.aboutpage)

    iv) views.py
        import render, HttpResponse
        def homepage(request):
            return HttpResponse('Welcome')

            run the server in terminal

        def aboutpage(request):
            return HttpResponse('This is About')

            run the server and check for url http://127.0.0.1:8000/about/

----------------------------------------------------------------------------------------------------------------------------------

1)Make a templates directory in login
make a html file named login.html and write
<form action='/login/verify/' method='POST'>
            Username:<input type='text' name='uname'/><br/>
            Password:<input type='password' name='pw'/><br/>
            <input type='submit' value='LOGIN'/>
            </form>

Go to login/views.py
type return render(request,'login.html',{}) instead of return httpresponse

2)We need to create table using model
Go to login/model.py
write
from django.db import models
class AccountModel(models.Model):
    user=models.CharField(max_length=100)
    pwd=models.CharField(max_length=100)
    class Meta:
        db_table='user_details'

Then run these lines
python manage.py makemigrations
python manage.py migrate
then check in db browser a table is made

3)We want this table to be viewed on admin page
go to login/admin.py
write
from .models import AccountModel
admin.site.register(AccountModel)
Go to /admin and u can see model there and add one username and passsword


4)Now add this to model.py
    def __str__(self): #This is written as if we make any changes to Model we dont have to write migrations
        return self.user

5)A username and password will be searched in db and if not found we will add one
Add path('verify/',views.verifypage) to login/urls path
We are getting an error CSRF thts becoz it creates a new token so we add
{%csrf_token%} after form tag in html file
Now no error will be there


6)Now we will try to retrive username and password and if it is not present in db we create one
Go to login/views
Comment out the httpresponse and write
def verifypage(request):
    #return HttpResponse('Verified')
    if request.method=='POST':
        u=request.POST.get('uname')
        p = request.POST.get('pw')
        from .models import AccountModel as AM
        try:
            AM.object.get(user=u)
            return HttpResponse('Login Success')
        except:
            a=AM()
            a.user=u
            a.pwd=p
            a.save()
            return HttpResponse('Account Created ')

 

