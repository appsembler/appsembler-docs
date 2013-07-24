****************************************
Adding New Relic monitoring to OpenShift
****************************************

1. **Install New Relic package**
   
   You'll want to add the New Relic package to you requirements.txt file:

   .. code-block:: sh
   
       echo "newrelic" >> requirements.txt
   
2. **Initialize the config file**
   
   Once the New Relic package is installed, either during the deploy or manually using pip, 
   SSH into your OpenShift gear, activate the virtual environment and initialize the config file:

   .. code-block:: sh
   
       rhc ssh <app_name>
       source python/virtenv/bin/activate
       cd app-root/data
       newrelic-admin generate-config <your-license-key> newrelic.ini

3. **Change the app name in the config file**
   
   The app name defines how the app is going to show in the New Relic site:

   .. code-block:: ini
   
        # The appplication name. Set this to be the name of your
        # application as you would like it to show up in New Relic UI.
        # The UI will then auto-map instances of your application into a
        # entry on your home dashboard page.
        app_name = YourAppName

4. **Initialize New Relic library in the WSGI script**
   
   Add the following code to your wsgi script in `wsgi/application` in your repo 
   above the part where WSGI application is created:

   .. code-block:: python
   
        try:
            import newrelic.agent

            config_file = os.path.join(os.environ['OPENSHIFT_DATA_DIR'], 'newrelic.ini')
            if os.path.exists(config_file):
                newrelic.agent.initialize(config_file, 'production')
        except IOError:
            pass

5. **(Optional) add New Relic support for Celery**
   
   Add the following code to your .openshift/action_hooks/post_deploy script:

   .. code-block:: sh
   
       NEW_RELIC_CONFIG_FILE=${OPENSHIFT_DATA_DIR}newrelic.ini newrelic-admin run-python "$OPENSHIFT_REPO_DIR"path-to-your-django-app/manage.py celeryd -l warning -B --autoscale=6,3 -s "$OPENSHIFT_DATA_DIR"celerybeat --settings=your-django-app.settings.production >> $OPENSHIFT_PYTHON_LOG_DIR/celery.log 2>&1 &
