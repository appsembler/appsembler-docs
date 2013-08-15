****************************************
Deploy procedure for Mediathread
****************************************

The whole deploy process is organized around 3 repositories:

1. Mediathread Django app (https://github.com/appsembler/mediathread)
2. Mediathread Openshift wrapper (https://github.com/appsembler/mediathread-openshift-quickstart)
3. Openshift app repo

The deploy process will be explained on an example. For instance, you need to make a change to a file
in the Mediathread app:

1. Make the change to the file in the Mediathread Django app repo
2. Commit changes and push to Github
3. Go to your ``mediathread-openshift-quickstart`` repo and do:
   
   .. code-block:: sh
   
        cd mediathread
        git pull
        git commit -am "Updated submodule"
        git push

4. Next, go to your Openshift app repo (the one where you do a git push to production) and
   do

   .. code-block:: sh
   
        git pull -X theirs upstream master
        git push
