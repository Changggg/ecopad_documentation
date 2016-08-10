FURTHER DEVELOPMENT
=====================


Further development can be of two types:-
   * Adding a new task to the existing model
    
   * Adding a new model


Note :- It's a very bad practice to modify existing working codes in github master branch and push it back to the master node again.

    1. First create new branch ecopadq.Push codes to the new branch.This is done for version control. 

    2. Create a new task with @task() method decorator

    3. Within celery folder update requirements.txt to include ecopadq test branch instead of master

    4. Restart system
    
    5. Check whether new or updated tasks works fine or not
    
    6. If everything goes well merge branch to master.

Adding a new task to the existing model
--------------------------------------------

Inorder to add a new task there are two ways:-
   * to an exiting model-go to folder （my_cybercommons/celery/env/lib/python2.7/site-packages/ecopadq/tasks）and a create a python file which will have the fuction required for the file and we need to add (@task) before the begining of the function.Then inside the same folder open the __init.py file and then import your new task.
   
    `# from your_file_name_without_extension import *`
  
   * add the function to the existing task.py file and add (@task) before the begining of the function. 
 

Adding a new model
---------------------

Inorder to add a new model,we first need to build the model and also a docker file which will contain all the path information of the inputs required for the model and also install all the dependency softwares required to run the model in the proper environment.Then goto （my_cybercommons/celery/env/lib/python2.7/site-packages/ecopadq/tasks）and a create a python file which will have the fuction required for the file and we need to add (@task) before the begining of the function or  add the function to the existing task.py file and add (@task) before the begining of the function.
