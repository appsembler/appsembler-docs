*********************************
How to set up Sentry on OpenShift
*********************************

1. **Start a Sentry instance**
   
   .. code-block:: sh
   
        rhc app create --scaling <app_name> python-2.6 mongodb-2.2 postgresql-8.4
        cd <app_name>
        git remote add upstream -m master git://github.com/zemanel/openshift-sentry-quickstart.git
        git pull -s recursive -X theirs upstream master
        git push origin

2. **Get the API key**
   
   Log in into your deployed Sentry instance, create a project and find the 
   Client configuration->Python option in the settings and copy the `RAVEN_CONFIG` 
   setting. It should look something like this:

   .. code-block:: python
   
        # Set your DSN value
        RAVEN_CONFIG = {
            'dsn': 'https://sdasfdgfdjng342rewfsd@sentryweb-namespace.rhcloud.com/2',
        }

3. **Install raven library**
   
   Add the raven library to your requirements.txt:

   .. code-block:: sh
   
        echo "raven" >> requirements.txt

4. **Set the DSN value in settings.py**
   
   Paste the `RAVEN_CONFIG` setting you got from the Sentry site to your settings.py file

5. **Add raven to installed apps**
   
   .. code-block:: python
   
        # Add raven to the list of installed apps
        INSTALLED_APPS = INSTALLED_APPS + (
            # ...
            'raven.contrib.django.raven_compat',
        )

6. **(Optional) Integrate django logging with Sentry**
   
   Paste this in your settings.py and tweak it to your requirements:

   .. code-block:: python
   
        LOGGING = {
        'version': 1,
        'disable_existing_loggers': True,
        'root': {
            'level': 'WARNING',
            'handlers': ['sentry'],
        },
        'formatters': {
            'verbose': {
                'format': '%(levelname)s %(asctime)s %(module)s %(process)d %(thread)d %(message)s'
            },
        },
        'handlers': {
            'sentry': {
                'level': 'ERROR',
                'class': 'raven.contrib.django.raven_compat.handlers.SentryHandler',
            },
            'console': {
                'level': 'DEBUG',
                'class': 'logging.StreamHandler',
                'formatter': 'verbose'
            }
        },
        'loggers': {
            'django.db.backends': {
                'level': 'ERROR',
                'handlers': ['console'],
                'propagate': False,
            },
            'raven': {
                'level': 'DEBUG',
                'handlers': ['console'],
                'propagate': False,
            },
            'sentry.errors': {
                'level': 'DEBUG',
                'handlers': ['console'],
                'propagate': False,
            },
        },
    }