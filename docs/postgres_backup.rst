****************************************
Setting up a periodic PostgreSQL DB dump
****************************************

1. **Add cron cartridge**
   
   In order to run periodic tasks, be sure to add the cron cartridge
   to your app:

   .. code-block:: sh
   
       	rhc cartridge add cron-1.4 -a <app_name>

   *Note: you currently can't add the cron cartridge to scaled apps*

2. **Set up cron script**
   
   Add the following code to your `.openshift/cron/daily` folder (or hourly) 
   if you want more frequent backups:

   .. code-block:: sh
   
       	#!/bin/bash
		# Backs up the OpenShift PostgreSQL database for this application
		# by Skye Book <skye.book@gmail.com>
		 
		NOW="$(date +"%Y-%m-%d")"
		FILENAME="$OPENSHIFT_DATA_DIR/$OPENSHIFT_APP_NAME.$NOW.backup.sql.gz"
		pg_dump -h $OPENSHIFT_POSTGRESQL_DB_HOST -p $OPENSHIFT_POSTGRESQL_DB_PORT -F c $OPENSHIFT_APP_NAME > $FILENAME

	This will dump the DB to a file in your OpenShift data directory, so have some way
	of getting these files to a secure location.
