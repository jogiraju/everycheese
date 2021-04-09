EveryCheese
==============================

The Ultimate Cheese Index!

### Quick setup

> The next steps assume that conda is already installed

1 - <a name="step-1">Create a conda environment:</a>


```bash
conda create python=3.8 -n everycheese
```
2 - <a name="step-2">Activate the conda environment</a>

```bash
conda activate everycheese
```

3 - <a name="step-3">Install the project basic dependencies and development dependencies</a>

> Make sure you are inside the root project directory before executing the next commands.
>
> The root project directory is the directory that contains the `manage.py` file

On Linux and Mac

```bash
pip install -r requirements/local.txt
```

On Windows

```bash
pip install -r requirements\local.txt
```

4 - <a name="step-4">Configure the database connection string on the .env</a>

On Linux and Mac

```bash
cp env.sample.mac_or_linux .env
```

On Windows

```bash
copy env.sample.windows .env
```

Change the value of the variable `DATABASE_URL` inside the file` .env` with the information of the database we want to connect.

Note: Several project settings have been configured so that they can be easily manipulated using environment variables or a plain text configuration file, such as the `.env` file.
This is done with the help of a library called django-environ. We can see the formats expected by `DATABASE_URL` at https://github.com/jacobian/dj-database-url#url-schema. 

5 - <a name="step-5">Use the django-extension's `sqlcreate` management command to help to create the database</a>

On Linux:

```bash
python manage.py sqlcreate | sudo -u postgres psql -U postgres
```

On Mac:

```bash
python manage.py sqlcreate | psql
```

On Windows:

Since [there is no official support for PostgreSQL 12 on Windows 10](https://www.postgresql.org/download/windows/) (officially PostgreSQL 12 is only supported on Windows Server), we choose to use SQLite3 on Windows

6 - <a name="step-6">Run the `migrations` to finish configuring the database to able to run the project</a>


```bash
python manage.py migrate
```


### <a name="running-tests">Running the tests and coverage test</a>


```bash
coverage run -m pytest
```


## <a name="troubleshooting">Troubleshooting</a>

If for some reason you get an error similar to bellow, is because the DATABASE_URL is configured to `postgres:///everycheese` and because of it the generated `DATABASES` settings are configured to connect on PostgreSQL using the socket mode.
In that case, you must create the database manually because the `sqlcreate` is not capable to correctly generate the SQL query in this case.

```sql
ERROR:  syntax error at or near "WITH"
LINE 1: CREATE USER  WITH ENCRYPTED PASSWORD '' CREATEDB;
                     ^
ERROR:  zero-length delimited identifier at or near """"
LINE 1: CREATE DATABASE everycheese WITH ENCODING 'UTF-8' OWNER "";
                                                             ^
ERROR:  syntax error at or near ";"
LINE 1: GRANT ALL PRIVILEGES ON DATABASE everycheese TO ;
```



```sql
ERROR:  role "myuser" already exists
ERROR:  database "everycheese" already exists
GRANT
```

<a name="troubleshooting-delete-database">You can delete the database and the user with the commands below and then [perform step 5 again](#step-5).</a>

> :warning: **Be very careful here!**: The commands below erase data, and should only be executed on your local development machine and **NEVER** on a production server.


On Linux:

```bash
sudo -u postgres dropdb -U postgres --if-exists everycheese
sudo -u postgres dropuser -U postgres --if-exists myuser
```

On Mac:

```bash
dropdb --if-exists everycheese
dropuser --if-exists myuser
```

Additional notes:

This is the default, which is used by local.py for local development. If we open up local.py, we’ll see that there is no DATABASES setting in there, and so it defaults to the above. In production, we do not provide a default, as we can see in production.py. We have to handle that ourselves. 

pip install -r requirements/local.txt
-   Installs recursively all the packages listed in local.
    For production use install requirements/production.txt.

python manage.py migrate
This step gave error saying that no password passed.
So the configuration file base.py has been changed :
DATABASES = {
    # Raises ImproperlyConfigured Exception
    # if DATABASE_URL Not in os.environ and
    # the "default" argument is not defined.
    # The DATABASE_URL environment variables
    # expect a value in the following format:
    # DATABASE_URL=postgres://user:password@hostname_or_ip:port/database_name
    #"default": env.db(
    #    "DATABASE_URL",
    #    default="postgres:///everycheese",       
    #)
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'everycheese',
        'USER': 'postgres',
        'PASSWORD': 'dspaceworldPassword for postgres user',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
python manage.py runserver
Open a web browser to 127.0.0.1:800018. 
I could see a barebones EveryCheese website running. 

Git repository everycheese is opened.
From insed everycheese directory ran:
git init
(everycheese) F:\django-crash-course\everycheese>git remote add origin https://github.com/jogiraju/everycheese.git

(everycheese) F:\django-crash-course\everycheese>git add .

(everycheese) F:\django-crash-course\everycheese>git commit -m "Boilerplate files generated from the `django-crash-starter` project template"
[master (root-commit) e066d58] Boilerplate files generated from the `django-crash-starter` project template
 86 files changed, 3424 insertions
(everycheese) F:\django-crash-course\everycheese>git push -u origin master
Fatal: HttpRequestException encountered.
Username for 'https://github.com': jogiraju
Password for 'https://jogiraju@github.com':
Counting objects: 102, done.
(everycheese) F:\django-crash-course\everycheese>git log
commit e066d58af321e2d89f7c3f5fa9d60f91314aa133
Author: jogiraju <rajujogi.t@gmail.com>
Date:   Fri Apr 9 17:17:40 2021 +0530

    Boilerplate files generated from the `django-crash-starter` project template

python manage.py runserver

Go to http://127.0.0.1:8000 to see the starter EveryCheese website. Click on the “Sign Up” link in the navbar. 
Enter these values:
• email: cheesehead@example.com
• username: cheesehead 
• password: *QjkXCPUm8iuaeP
 
We’re not going to get an email confirmation by regular email. django-crash-starter isn’t set up to send out real emails during local development, and for good reason. We wouldn’t want to accidentally send out emails to real people during local development. However, django-crash-starter has configured our project to display all outgoing emails in the console, where runserver is running. 

views.py
class UserDetailView(LoginRequiredMixin, DetailView):
    model = User
    # These Next Two Lines Tell the View to Index
    #   Lookups by Username
    slug_field = "username"
    slug_url_kwarg = "username"
This is update view part of My Info when the user logs in http://localhost:8000.
Some things to notice:
• UserDetailView is a subclass of Django’s generic DetailView. • The view can only be accessed by logged-in users because of LoginRequiredMixin. • We have to explicitly specify the model with model = User. • Note the URL /users/cheesehead/ and how username is used as a slug in the URL.

<a class="btn btn-primary" href="{% url 'users:update' %}"> My Info</a> 
<a class="btn btn-primary" href="{% url 'account_email' %}"> E-Mail</a>
Both buttons are actually just HTML links that are styled to look like buttons. The url tag is a built-in template tag that converts a URL name to an absolute path URL. 

The My Info button points to a URL named update within the users namespace, which is wired up with UserUpdateView in the users/urls.py module. 

The E-Mail button link points to a URL named account_email. That URL is defined in allauth’s urls.py file, which we can find at https://github.com/pennersr/django-allauth/blob/master/allauth/ account/urls.py.

django-crash-starter came with django-allauth pre-configured with its users app. Since it’s a dependency of the everycheese project that we generated, links in users app templates can go to URLs defined by allauth.

Add this to the User model in the users app models.py:
bio = models.TextField("Bio", blank=True)

We can put that line right below the name field.
Then make and apply our migrations as usual:
python manage.py makemigrations users python manage.py migrate users

