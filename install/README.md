Installation
==============

Inorder to install the cyberCommons platform on a single node,we need to do it through cookiecutter which is like a bash script to install the cyberCommons platform and link the various docker containers together.

##### Dependencies Requirements
1. Install Docker
   * Inorder to install the cyberCommons platform,you first need to install docker.
   To learn more about docker and install it [click here](https://docs.docker.com/engine/installation/)

2. Install cookiecutter
    * `# pip install cookiecutter`


To the know more about cookiecutter and installing the cyberCommons platform [click here](https://github.com/cybercommons/cybercom-cookiecutter)


More Specific Additional Installation
----------------------
Note:- Don't run any of the following command as root user

Let us assume that the folder name of docker_host_data_directory is [[application_short_name]] which you have named while installing the cyberCommons platform in our local machine.

#### Building the teco_spruce and teco_spruce_viz images

1. Git clone teco_spruce and teco_spruce_viz from github inside [[application_short_name]]
   

     `# git clone https://github.com/ou-ecolab/teco_spruce`

     `# git clone https://github.com/ou-ecolab/teco_spruce_viz`

       teco_spruce folder contains the actual fortran code which runs the teco_spruce model.This folder also contains a DockerFile using which we can build an image of teco_spruce.

       teco_spruce_viz contains the R code for the visualization of the generated graphs.This folder also contains a DockerFile using which we can build an image of teco_spruce_viz.


2. Goto （application_short_name） folder and create a folder  [[misc_data]] and go inside that folder and copy some important which will be required during the course of the installation. 
   
     `# mkdir misc_data`

     `# cd misc_data`

     `#  wget -r -np -nd --reject "index.html*" http://ecolab.cybercommons.org/misc/spruce_data/ wget -r -np -nd --reject "index.html*" http://ecolab.cybercommons.org/misc/spruce_data/ `
     
3. Create a folder inside [[misc_data]] folder and name it [[Weathergenerate]] and move all the csv file from [[misc_data]] to it.
     
     `# mkdir Weathergenerate`

     `# mv EMforcing* Weathergenerate/ `
     
     The Weathergenerate file is needed to build  the teco_spruce image.

4. Goto [[application_short_name/teco_spruce/input]] folder and copy the Weathergenerate file into it.

      `#cp -r path__to_application_short_name/misc_data/Weathergenerate .`
      
3. Goto [[application_short_name/teco_spruce]] and build the  teco_spruce image.

     `# docker build -t teco_spruce .`

4. Goto （application_short_name/teco_spruce_viz） folder and inside it build ecopad_r image.

     `# docker build -t ecopad_r .`

#### Creating the spruce_data folder

5. Goto （application_short_name/data/local） and create a folder spruce_data.

     `#mkdir spruce_data`

       This folder will contain all the input files required to run  the teco_spruce model.

6. Inside spruce_data copy Weathergenerate folder,SPRUCE_da_pars.txt,SPRUCE_forcing.txt,SPRUCE_obs.txt and SPRUCE_pars.txt  from          （application_short_name/teco_spruce/input） folder.These files are required to run the fortran code inside teco_spruce. 
 
        `# cd spruce_data `

        `# cp -r /path_to_application_short_name/teco_spruce/input/* .`

7. Copy the initial.txt file from [[application_short_name/misc_data]] inside spruce_data.
 
       `# cp -r /path_to_application/misc/initial.txt .`

       The initial.txt contains data from spruce website for year(2011-2015)

#### Downloading the frontend contents

8. Git clone ecopad_portal from github inside （application_short_name/data/static） folder.
 
       `# git clone https://github.com/ou-ecolab/ecopad_portal`

       ecopad_portal contains the index.html along whith the api.js and template files which is the Grapical User Interface(GUI) of the system.

#### Some additional configurations 


10. Add pandas to the requirment.txt inside （application_short_name/celery/code）.

       Requirement.txt contains all the dependent libraries required to run run our system.Pandas is a python library which is used in the system to pull data from the spruce website.To learn more about pandas [click here](http://pandas.pydata.org/)

11. Inside [[application_short_name/celery/code]] create a folder  task_config and inside task_config create a file config.py. config.py contain the username and password required to pull data from spruce_data website.

      `# vi config.py`
      
      `#ftp_username="<username>"` 
      
      `#ftp_password="<password>" `
 `

12. Create a (ecopad_tasks) folder inside （application_short_name/data/static) and inside it create a (default) folder  and inside (default) folder  copy Paraest.txt from （application_short_name/teco_spruce/output）and SPRUCE_da_pars.txt from （application_short_name/teco_spruce/input）
    
       `# mkdir ecopad_tasks`

       `# cd ecopad_tasks `

       `# mkdir default`
       
       `# cd default`
       
       `# cp /path_to_application_short_name/teco_spruce/output/Paraest.txt .`
    
       `# cp /path_to_application_short_name/teco_spruce/input/SPRUCE_da_pars.txt .`
