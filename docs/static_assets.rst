**********************************************
Serving Django static assets on from OpenShift
**********************************************

1. **Create a collected_static folder during build**
   
   Add the following to your ``.openshift/action_hooks/build`` script:

   .. code-block:: sh
   
        if [ ! -d $OPENSHIFT_REPO_DIR/wsgi/static/collected_static ]; then
            mkdir $OPENSHIFT_REPO_DIR/wsgi/static/collected_static
        fi

2. **Correctly set up Django settings**
   
   Make sure your ``STATIC_ROOT``, ``STATIC_URL`` and ``STATICFILES_DIR`` settings are correctly
   set. They should look something like this:

   .. code-block:: python
   
        STATIC_ROOT = os.path.join(get_env_variable('OPENSHIFT_REPO_DIR'), 'wsgi', 'static', 'collected_static')
        STATIC_URL = '/static/'
        STATICFILES_DIRS = (
            os.path.join(PROJECT_ROOT, 'static'),
        )       

3. **Set up apache mod_rewrite**
   
   Add the following lines to your ``wsgi/.htaccess`` file:

   .. code-block:: apache
   
        RewriteEngine on
        RewriteRule ^application/site_media/(.+)$ /static/collected_static/$1 [L]

4. **Run collectstatic in deploy script**
   
   Add the following somewhere at the end of your ``.openshift/action_hooks/deploy`` script:

   .. code-block:: sh
   
        export DJANGO_SETTINGS_MODULE=your.settings.module
        echo "Executing 'django-admin.py collectstatic'"
        django-admin.py collectstatic --noinput --settings=$DJANGO_SETTINGS_MODULE



Note that the ``get_env_variable`` mentioned above is defined in the settings.py as:

.. code-block:: python

        from django.core.exceptions import ImproperlyConfigured


        def get_env_variable(var_name):
            """ Get the environment variable or return exception """
            try:
                return os.environ[var_name]
            except KeyError:
                error_msg = "Set the %s environment variable" % var_name
                raise ImproperlyConfigured(error_msg)
