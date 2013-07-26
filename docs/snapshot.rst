**********************************************
Saving and restoring applications on OpenShift
**********************************************

1. **Taking a snapshot**
   
   First, you need to take an archived snapshot of the current running application:

   .. code-block:: sh
   
       	rhc snapshot save -a <app_name>

2. **Restoring a snapshot**
   
   You have to create a new application using the same cartridges as the saved app,
   either on a different account or on the same after you delete your app using
   `rhc app delete <app_name>`. Do so by using the following:

   .. code-block:: sh
   
       	rhc snapshot restore -a <app_name> -f <path/to/your/archive.tar.gz>

*Note: the app name must be the same on both the old and new apps for the
process to work*

*Note #2: you can run into problems restoring except with the most simple apps
so be sure to have some other way of backing up the data on the app*
