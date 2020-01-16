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



