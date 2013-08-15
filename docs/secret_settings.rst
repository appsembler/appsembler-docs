**********************************
Handling secret settings in Django
**********************************

When deploying apps somewhere, often there's a need to remove secret data from
the settings in the repo you keep on GitHub or somewhere else. This document will
show you how to handle secret settings when deploying to OpenShift.

1. **Create a secret_keys file**
   
   You'll need to create a secret_keys file in which you'll set and export the secret
   settings as shell enviromental variables. The file should look similar to this one:

   .. code-block:: sh
   
        #!/bin/bash

        # Django settings
        export SECRET_KEY='yyyyyyy'

        # Email settings
        export MANDRILL_API_KEY='xxxxxxxx'

        # Sentry settings
        export SENTRY_DSN='https://xxxx:yyyy@domain.com/2'

        # Pusher settings
        export PUSHER_APP_ID='xxxxx'
        export PUSHER_APP_KEY='yyyyy'
        export PUSHER_APP_SECRET='zzzzz'

        # OpenShift settings
        export OPENSHIFT_USER='user@domain.com'
        export OPENSHIFT_PASSWORD='secretpassword'

        # New Relic settings
        export NEW_RELIC_CONFIG_FILE='newrelic.ini'
        export NEW_RELIC_ENVIRONMENT='production'

    Put that file in the root of your OpenShift app repository (the one you see when
    you do ``rhc app show``).

2. **Copy the secret_keys file to the server**
   
   Run the following commands to add the secret_keys to the OpenShift app repo.

   .. code-block:: sh
   
        git add secret_keys
        git commit -m "Added secret_keys file"       
        git push

3. **Source secret_keys during deploy**
   
   Be sure to source the secret_keys file in every action hook on OpenShift where
   you have to run something related to Django. To be safe, it would be best to simply
   source it in all action hooks like this:

   .. code-block:: sh
   
        source ${OPENSHIFT_REPO_DIR}secret_keys
