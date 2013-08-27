****************************************
Setting up a periodic PostgreSQL DB dump
****************************************

1. **Add cron cartridge**
   
   In order to run periodic tasks, be sure to add the cron cartridge
   to your app:

   .. code-block:: sh
   
        rhc cartridge add cron-1.4 -a <app_name>

   *Note: you currently can't add the cron cartridge to scaled apps*

2. **Download and copy s3cmd to your OpenShift data folder**
   
   Download s3cmd from http://s3tools.org/s3cmd, copy it to OpenShift and unpack it:

   .. code-block:: sh

     scp s3cmd-1.5.0-alpha3.tar.gz openshiftusername@address:app-root/data
     rhc ssh <app_name>
     cd app-root/data
     tar xzvf s3cmd-1.5.0-alpha3.tar.gz
     rm s3cmd-1.5.0-alpha3.tar.gz
     mv s3cmd-1.5.0-alpha3 s3cmd
     cd s3cmd

   Now run the following command and put in your Amazon S3 secret keys:

   .. code-block:: sh

     s3cmd --configure -c .s3cfg 


3. **Set up cron script**
   
   Add the following code to your ``9.openshift/cron/daily`` folder (or hourly 
   if you want more frequent backups):

   .. code-block:: sh
   
    #!/bin/bash
    # Backs up the OpenShift PostgreSQL database for this application
    # by Skye Book <skye.book@gmail.com>
 
    NOW="$(date +"%Y-%m-%d")"
    FILENAME="$OPENSHIFT_DATA_DIR/db_backups/$OPENSHIFT_APP_NAME.$NOW.backup.sql.gz"
    /opt/rh/postgresql92/root/usr/bin/pg_dump -h $OPENSHIFT_POSTGRESQL_DB_HOST -p $OPENSHIFT_POSTGRESQL_DB_PORT -F c $OPENSHIFT_APP_NAME > $FILENAME
    cd $OPENSHIFT_DATA_DIR/s3cmd 
    s3cmd -c .s3cfg put $FILENAME s3://mediathreadbackups
