### using apt installing apache2,python3,python3-pip/pip3,python3-venv
### creating python virtual environment using command "python -m venv python-env-name"
### activating venv using command "source python-env-name/bin/activate"
### using pip3 installing django with "pip3 install django"
### creating django project with "django-admin startproject djangoproject ." # . for creating project in this dir without making extra one dir
### creating app with "python3 manage.py startapp djangoapp"
##  Running the Django Project with "python3 manage.py runserver"
## to run our application by using apache server , we need to configure apache2.conf file located at /etc/apache directory. Add the following code into this file.
    WSGIScriptAlias / path/to/wsgi.py  
    WSGIPythonPath path/to/project's/home 
      
    <Directory path/to/project's/home>  
       <Files wsgi.py>  
                    Require all granted  
       </Files>  
    </Directory>  
### but you will get error to sun apache2 server.
Starting apache2.service - The Apache HTTP Server...
apache2.service: Control process exited, code=exited, status=1/FAILURE apache2.service: Failed with result 'exit-code'.
AH00526: Syntax error on line 182 of /etc/apache2/apache2.conf:
Invalid command 'WSGIScriptAlias', perhaps misspelled or defined by a module not included in the server configuration
Failed to start apache2.service - The Apache HTTP Server.
## to get rid of this problem .
### running the command "apt-get install libapache2-mod-wsgi-py3"
### _I got Internal Server Error_ I failed with this

### https://www.digitalocean.com/community/tutorials/how-to-serve-django-applications-with-apache-and-mod_wsgi-on-ubuntu-14-04

/etc/apache2/sites-available/000-default.conf
<VirtualHost *:80>
    . . .

    Alias /static /home/user/myproject/static
    <Directory /home/user/myproject/static>
        Require all granted
    </Directory>

    <Directory /home/user/myproject>
        <Files wsgi.py>
            Require all granted
        </Files>
    </Directory>

    WSGIDaemonProcess myproject python-path=/home/user/ python-home=/home/user/penv
    WSGIProcessGroup myproject
    WSGIScriptAlias / /home/user/djangoproject/wsgi.py

</VirtualHost>
### ###
chmod 664 ~/myproject/db.sqlite3
chown :www-data ~/myproject/db.sqlite3
chown :www-data ~/myproject



