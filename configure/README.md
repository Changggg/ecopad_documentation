Configuration
==============



Docker Configure
-----------------

1. Create a docker group

      `# sudo groupadd docker`

2. Add your username to the docker group.

      ` # sudo usermod -aG docker <user_name> ` 
      
3. Run this command so that you don't need to restart your shell for the change to occur.

      `# sg docker -c "bash"`

System Configuration
----------------------
Note:- Don't run any of the following command as root user

Let us assume that the folder name of docker_host_data_directory is (my_cybercommons) which you have named while installing the cyberCommons platform in our local machine.

#### Building the teco_spruce and teco_spruce_viz images

1. Git clone teco_spruce and teco_spruce_viz from github inside (my_cybercommons)
   

     `# git clone https://github.com/ou-ecolab/teco_spruce`

     `# git clone https://github.com/ou-ecolab/teco_spruce_viz`

       teco_spruce folder contains the actual fortran code which runs the teco_spruce model.This folder also contains a DockerFile using which we can build an image of teco_spruce.

       teco_spruce_viz contains the R code for the visualization of the generated graphs.This folder also contains a DockerFile using which we can build an image of teco_spruce_viz.


2. Goto （my_cybercommons/teco_spruce/input） folder and copy the Weathergenerate folder from the server to this location. 
   
     `# scp -r <your username>@ecolab.cybercommons.org:/home/ecopad/ecopad/teco_spruce/input/Weathergenerate .`


       The Weathergenerate file is needed to build  the teco_spruce image.


3. Goto （my_cybercommons/teco_spruce） folder and inside it build teco_spruce image.

     `# docker build -t teco_spruce .`

4. Goto （my_cybercommons/teco_spruce_viz） folder and inside it build ecopad_r image.

     `# docker build -t ecopad_r .`

#### Creating the spruce_data folder

5. Goto （my_cybercommons/data/local） and create a folder spruce_data.

     `#mkdir spruce_data`

       This folder will contain all the input files required the teco_spruce model.

6. Inside spruce_data copy Weathergenerate folder,SPRUCE_da_pars.txt,SPRUCE_forcing.txt,SPRUCE_obs.txt and SPRUCE_pars.txt  from          （my_cybercommons/teco_spruce/input） folder.These files are required to run the fortran code inside teco_spruce. 
 
       `# cp -r ../../../teco_spruce/input .`

7. Copy the initial.txt file from the server inside spruce_data.
 
       `# scp -r <your username>@ecolab.cybercommons.org:/home/ecopad/ecopad/data/local/spruce_data/initial.txt .`

       The initial.txt contains data from spruce website for year(2011-2015)

#### Downloading the frontend contents

8. Git clone ecopad_portal from github inside （my_cybercommons/data/static） folder.
 
       `# git clone https://github.com/ou-ecolab/ecopad_portal`

       ecopad_portal contains the index.html along whith the api.js and template files which is the Grapical User Interface(GUI) of the system.

#### Some additional configurations 

9. Goto （my_cybercommons/celery） and creat an env folder inside it.This env folder will downland all the required dependecies and store in the local system so that you don't need to download again and again.

       `# mkdir env`

10. Add pandas to the requirment.txt inside （my_cybercommons/celery/code）.

       Requirement.txt contains all the dependent libraries required to run run our system.Pandas is a python library which is used in the system to pull data from the spruce website.To learn more about pandas [click here](http://pandas.pydata.org/)

11. Create a config.py file in （my_cybercommons/celery/env/lib/python2.7/site-packages/ecopadq/tasks） folder.This file will contain username and password to download data from spruce_data website.

    `# ftp_uername=<give username>`
    
    `# ftp_password=<give password>`

12. Create a (default) folder inside ecotest/data/static/ecopad_tasks and copy Paraest.txt from （my_cybercommons/teco_spruce/output）and SPRUCE_da_pars.txt from （my_cybercommons/teco_spruce/input）

       `# mkdir default`
    
       `# cp ../../../teco_spruce/output/Paraest.txt .`
    
       `# cp ../../../teco_spruce/input/SPRUCE_da_pars.txt .`

#### Creating authentication keys

13. Now we need to create ssh keys which will create 3 files id_rsa,id_rsa.pub and known_hosts.From the id_rsa.pub file,we will again create another file known as authorizeed_keys.These keys are required to connect to the cybercommons platform.To create the keys type the following commands.

    `# ssh-keygen`
    
       Press Enter for the first command ,don't type any paraphase for second and third commands.After that type the following command
    
    `# cat id_rsa.pub >> authorized_keys`

       To run the system we need keys to communicate with the system.This is the reason why we create ssh keys.To learn more about ssh keys [click here](https://help.github.com/articles/generating-an-ssh-key/)

#### Configuring the cybercom_up file and running the system

14. Now go to （cybercommon/run/） and open the file cybercom_up and in the celery part mount env,.ssh and add the environmental          variable host_data_dir in cybercom_up.
    
    `# vi cybercom_up`
    
       This is how the docker command of celery should exactly look like.

       
       docker run -d --name ecotest_celery --link ecotest_rabbitmq --link ecotest_mongo -v /home/<username>/.ssh:/root/.ssh -v /home/<username>/<path_to_mycybercocommons>/celery/env:/env:z -v /home/<username>/<path_to_mycybercocommons>/celery/code:/code:z -v /home/<username>/<path_to_mycybercocommons>/celery/log:/log:z -v /home/<username>/<path_to_mycybercocommons>/data:/data:z -e "host_data_dir=/home/<username>/<path_to_mycybercocommons>/data" -e "docker_worker=$host_ip" -e "docker_username=$docker_username"  -e "C_FORCE_ROOT=true" -e "CELERY_CONCURRENCY=8" cybercom/celery

15. Now the configuration is almost complete.Now when we run the cybercom_up file.Everthing should work perfectly fine.Type the           following set of commands so that the system restart the docker containers.
   
    `# ./cybercom_up`

    `# ./docker_restart`

    `# ./cybercom_up`
 
 After this open your browser and type 0.0.0.0/ecopad_portal.You should see the system running.
 
 If you are still facing problems [click here](https://github.com/ou-ecolab/ecopad_documentation/tree/master/system_control) for  assistance.
   
