*****************************************
Setting up Flower monitoring on OpenShift
*****************************************

1. **Install Flower**
   
   .. code-block:: sh
   
       	echo "flower" >> requirements.txt

2. **Run Flower during your deploy**
   
   Put the following lines to your `.openshift/action_hooks/post_deploy` file and redeploy the app:

   .. code-block:: sh
   
       	echo "Starting flower"
		python "$OPENSHIFT_REPO_DIR"appsembler-launch/openshift_deploy/manage.py celery flower --settings=openshift_deploy.settings.production --port=5445 --address=${OPENSHIFT_PYTHON_IP} --broker=redis://:${REDIS_PASSWORD}@${OPENSHIFT_REDIS_HOST}:${OPENSHIFT_REDIS_PORT}/0 >> $OPENSHIFT_PYTHON_LOG_DIR/flower.log 2>&1 &

3. **Forward ports**
   
   On your local machine, run the following command to forward ports:

   .. code-block:: sh
   
       	rhc port-forward <app_name>

   Flower will be available at the adress 127.0.0.1:5445_ 
   
   .. _127.0.0.1:5445: http://127.0.0.1:5445
