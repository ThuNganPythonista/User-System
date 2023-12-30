# + User System And Custom User Models In Django


##             User System And Custom User Models In Django ##
 

### Introduction :
 
Django is a back-end framework written in the Python programming language. The fundamental concepts to learn in Django include settings, templates, sessions, URLs, views, models, REST API, web testing, and eventually deploying.
 
#### I) 	User :
 
The authentication system in Django will, by default, include all user accounts, including admin and all user accounts. Each account can have different permissions. For example, in a healthcare service, the first user account may manage all user accounts registered for vaccinations, the second user account may manage new patient accounts, and so on.



 Project Structure :

We are learning Django at the beginning and already know how to create a project and create an app within the project. Below, I will give an example of login functionality within an  'user'  app to illustrate for everyone.

First, make sure your project structure looks like this :



Authenticate and create accounts on admin site :


Next, we will import `authenticate` from the django module to verify user accounts’ information. This function will consider whether `username` and `password` is valid or not :

`from django.contrib.auth import authenticate`

Now, we will create a admin account to log in admin site of Django by the command :

`python3 manage.py createsuperuser`



This account allows you log in Admin site



Define views.py to return the result when user logs in :


Now we will write a function in views.py to define the simplest login form, it only returns a message 'login successful' or 'login failed'




With GET and POST method, I have explained on my Github repo :

https://github.com/ThuNganPythonista/Django-Hades 
(my own complete Django web)


The first one, GET method, when users click to the register button, for instance, it returns the register template (HTML). This is because the GET method will take data from the server.
The second one, POST method, when users have already filled in their information and press the button register, that information will be sent to the database. This is because the POST method will handle a request sent to the server.
=> class Login (View) class inherits View from Django's views module.
def get(self,request) it calls the method GET to handle HTTP. 
self indicates that this is the instance of the class. Other languages do not require "this" or "self", but Python requires. 
request,a module of Python, holds HTTP request data, so you can find more detail in Python lessons.
 return render(request=request, template_name="success.html") it renders an HTML template user/success.html and returns it as an HTTP response.

`username ` and `password` will use POST method, and the information will be verified by `authenticate` function
`if my_user is None : ` If it can not authenticate `username` and `password`
`return HttpResponse`: Return on the web a message
`Return render` : Return template HTML
In this case, it will return a message “Login unsuccessfully ” to inform users if their account is invalid and return a template such as home page or success page for them.

c)  HTML LogIn :
For example, this is my HTML for login :

Here, I request users to log in with their email and password.
I have two `input` tags inside the `form` tag with method `post`.
{% csrf_token  %} : is required when we are dealing with forms in Django to protect against Cross-Site Request Forgery (CSRF) attacks. It is compulsory in Django, so If not having, we will get the error :


Register User in the Django admin site :
We try to log in the by an account that does not exist


This is its result when I logged in an account that is not registered in Admin site:


Now, I will create an account at Django admin site so that we can log in successfully 
I will change to site by adding `admin` and log in this Django site
=> http://127.0.0.1:8000/admin/



My account has username that is CodeLofi 
My account has password that is @CodeLofi092727
*IMPORTANT : Django will automatically make password hashing password instead of raw password. We can not see how the password looks, but we can change the user's password by clicking the link `this form` in the Django admin site.


Here, I have two accounts that is Thungan27 and CodeLofi 


Back to LogIn Page, we will try to log in the account that we just created, and this is the output :


When I submit the login form, it returns the login page. Why ?
This is because the way we define the function in views.py :


Here, `my_user` is not None, so we will return the `login.html` page.
I logged into an existing account and I return Login Page.

II)  Permission view (decorators):
We will often encounter websites that require users to log in before they can view products or complete exercises. Therefore, `decorators` here will handle this. Only after the user logs in can they view the content that has been set up in the views.py file, which acts as a fence.
Let's set up a view in views.py :



We will import `decorators`


The function `@decorators.login_required(login_url=“/login/”)` : It has the meaning as its name suggests. This means that login must be required to view it.


Moving to urls.py, we will declare its url :



When accessing the URL http://localhost:8000/view-product/, it has not displayed the information of the view function. After successful login, it will lead us to the display page. In other words, we now have the right to view.


After logging in, we see the information of view_something function. It means that ưe have been granted authorization

	III) Custom User Model In Django :
The default of User in Django includes just some basic fields such as First Name, Last Name and Email Address as we see below :


However, we need to require more information in reality such as gender, age, address and avatar
To add new those fields, we need to import AbstractUser into models.py.
Create a class in model file :
Use AbstractUser to inherit and add some more fields of User
First of all, we need to import `AbstractUser` by the command `django.contrib.auth.models`. 
Next, we can set up a class in the model file to define additional fields, such as gender, add address, or age and customise the input format into an option choice for gender according to our preferences.
We will create class named MyUser that inherits AbstractUser :


The IntegerField allows the User to input numbers, while the CharField is commonly used for text input. Additionally, the CharField must be accompanied by max_length to limit the number of characters.

default : the default value for the field

choices : The first element in each tuple is the value that will be stored in the database. The second one will be indicated by Widget.


Register User in admin file :
First, moving to admin file in local environment and register the model that we just created :


Declare the model:
Step 1 : Declare in settings.py file
Add this one to your settings.py file :
AUTH_USER_MODEL = 'YourAppName.YourClassName'
=> AUTH_USER_MODEL = ‘CodeLofi.MyUser’
	
	Step 2 : Run makemigrations :
We just defined the model named MyUser, so we need to run `makemigrations` to create it, the result out like it :





Connect MySQL with Django :
Database connection is crucial in web programming. By default, MySQL will connect to the sqlite database. However, I will guide you on how to connect with DataGrip.

Step 1 : Download and open DataGrip (https://www.jetbrains.com/datagrip/download/#section=mac)

 We will use DataGrip for database management instead of PhpMyAdmin and Xampp

Step 2 : Change database from sqlite3 (default db Django) to MySQL


	
You can take this template in the link https://docs.djangoproject.com/en/5.0/ref/settings/
and then change its engine, name, user, password, host, port
Engine : mysql - we use database mysql
Name : database table we will create in the app DataGrip
User : root (default)
Password : root (default)
host : localhost (mysql webserver)
port : 3306 is mysql

Step 3 :  Connect the project to DataGrip
 After downloading DataGrip, we are going to open and add our project LofiDjango


Here, I added LofiDjango. I have 3 projects in total which have database

Step 3 : Check the form in admin Django site 
Moving to 127.0.0.1:8000/admin/CodeLofi/myuser/add/
We check whether our form has gender, age and address or not ?
It means that the form MyUser in your models.py works or not.
We created some more fields such as  gender, age and address in comparison with the its default fields (username, firstname, lastname and email address)


This is our result : 


Yes, we have successfully added new fields to our database : gender, age and address 

Other lessons :
+ User System And Custom User Models In Django: https://docs.google.com/document/d/16xz23zC0bmDl8Ar24rBtSJKoTDEWhCk9TOL_4a_Nfro/edit?usp=sharing
+ Apps Feature In Django Framework : https://www.facebook.com/groups/devoiminhdidauthe/permalink/23968231386153907/ 
        	+ GIT : https://github.com/ThuNganPythonista/GIT
        	+ AbstractUser & AbtractBaseUser: https://github.com/ThuNganPythonista/Django-AuthenticationSystem-Tutorial
          	+ Run DMOJ (open source-code) by MacOs (mapping between linux and macos): https://docs.google.com/document/d/1B3F-6FFDeXeV2ra5d1sLhV1nA_Mr_SlyJpRkIWOQNaw/edit?usp=sharing
        	+ Explain instructions setting DMOJ (this one I write for my organization) : https://docs.google.com/document/d/1xlKEmaXDh0TEMaNMXHijv4FMajxsY4zBUbfLdRflMNc/edit
	+ Build e-commerce Django web : https://github.com/ThuNganPythonista/Django-Hades
	+ Some videos on CodeLofi : https://github.com/ThuNganPythonista/Code-Lofi













