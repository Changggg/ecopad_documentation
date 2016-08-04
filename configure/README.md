Configuration
==============

Let us assume that the folder name of docker_host_data_directory is <my_cybercommons> which you have named while installing the cyberCommons platform in our local machine.

1. Git clone teco_spruce and teco_spruce_viz from github inside <my_cybercommons>
   

     `# git clone https://github.com/ou-ecolab/teco_spruce`

     `# git clone https://github.com/ou-ecolab/teco_spruce_viz`

2. Goto <my_cybercommons/teco_spruce/input> folder and copy the Weathergenerate folder from the server to this location. 
   
     `# scp -r <your username>@ecolab.cybercommons.org:/home/ecopad/ecopad/teco_spruce/input/Weathergenerate .`

3. Goto <my_cybercommons/teco_spruce> folder and inside it build teco_spruce image.

     `# docker build -t teco_spruce .`

4. Goto <my_cybercommons/teco_spruce_viz> folder and inside it build ecopad_r image.

     `# docker build -t ecopad_r .`

5.Goto <my_cybercommons/data/local> and create a folder spruce_data.

     `#mkdir spruce_data`





6. Inside spruce_data copy Weathergenerate folder,SPRUCE_da_pars.txt,SPRUCE_forcing.txt,SPRUCE_obs.txt and SPRUCE_pars.txt  from          <my_cybercommons/teco_spruce/input> folder.These files are required to run the fortran code inside teco_spruce. 
 
    `# cp -r ../../../teco_spruce/input .`

7. Copy the initial.txt file from the server inside spruce_data.
 
   `# scp -r <your username>@ecolab.cybercommons.org:/home/ecopad/ecopad/data/local/spruce_data/initial.txt .`

8. Git clone ecopad_portal from github inside <my_cybercommons/data/static> folder.
 
    `# git clone https://github.com/ou-ecolab/ecopad_portal`
