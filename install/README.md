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
 
       `# cp -r ../../../teco_spruce/input/* .`

7. Copy the initial.txt file from the server inside spruce_data.
 
       `# scp -r <your username>@ecolab.cybercommons.org:/home/ecopad/ecopad/data/local/spruce_data/initial.txt .`

       The initial.txt contains data from spruce website for year(2011-2015)

#### Downloading the frontend contents

8. Git clone ecopad_portal from github inside （my_cybercommons/data/static） folder.
 
       `# git clone https://github.com/ou-ecolab/ecopad_portal`

       ecopad_portal contains the index.html along whith the api.js and template files which is the Grapical User Interface(GUI) of the system.

#### Some additional configurations 

9. Goto （my_cybercommons/celery） and create an env folder inside it.This env folder will downland all the required dependecies and store in the local system so that you don't need to download again and again.

       `# mkdir env`

10. Add pandas to the requirment.txt inside （my_cybercommons/celery/code）.

       Requirement.txt contains all the dependent libraries required to run run our system.Pandas is a python library which is used in the system to pull data from the spruce website.To learn more about pandas [click here](http://pandas.pydata.org/)



12. Create a (ecopad_tasks) folder inside （my_cybercommons/data/static) and inside it create a (default) folder  and inside (default) folder  copy Paraest.txt from （my_cybercommons/teco_spruce/output）and SPRUCE_da_pars.txt from （my_cybercommons/teco_spruce/input）
    
       `# mkdir ecopad_tasks`

       `# cd ecopad_tasks `

       `# mkdir default`
       
       `# cd default`
       
       `# cp ../../../teco_spruce/output/Paraest.txt .`
    
       `# cp ../../../teco_spruce/input/SPRUCE_da_pars.txt .`
