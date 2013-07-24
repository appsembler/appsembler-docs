********************************
SSL for Django apps on OpenShift
********************************

1. **Enable secure cookies**
   
   Add the following lines to your settings.py file:

   .. code-block:: python

        SESSION_COOKIE_SECURE = True
        CSRF_COOKIE_SECURE = True

2. **Enable HTTPS on the server**
   
   Add the following lines to your wsgi/.htaccess file:

   .. code-block:: apache
   
        RewriteEngine on  

        RewriteCond %{HTTP:X-Forwarded-Proto} !https  
        RewriteRule .* https://%{HTTP_HOST}%{REQUEST_URI} [R,L]  

3. **Enable HTTPS in the WSGI script**
   
   Add the following lines to your wsgi/application file:

   .. code-block:: python
   
        # make django aware that SSL is turned on
        os.environ['HTTPS'] = "on"
