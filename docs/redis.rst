**************************************
How to add Redis to your OpenShift app
**************************************

1. **Add the Redis cartridge to your app**
   
   .. code-block:: sh
   
       	rhc add-cartridge http://cartreflect-claytondev.rhcloud.com/reflect\?github\=smarterclayton/openshift-redis-cart -a <app_name>

2. **Get Redis env variables**
   
   When you SSH into your gear, you can see Redis env variables such as host, port, username and password
   that you can use in your applications:

   .. code-block:: sh
   
       	rhc ssh <app_name>
       	env | grep -i redis | sort
