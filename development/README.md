FURTHER DEVELOPMENT
=====================

Further development can be of two types:-
   * Adding a new task to the existing model
    
   * Adding a new model

Adding a new task to the existing model
--------------------------------------------

Inorder to add a new task there are two ways:-
   * to an exiting model-go to folder （my_cybercommons/celery/env/lib/python2.7/site-packages/ecopadq/tasks）and a create a python file which will have the fuction required for the file and we need to add (@task) before the begining of the function.
  
   * add the function to the existing task.py file and add (@task) before the begining of the function. 
 

Adding a new model
---------------------

Inorder to add a new model,we first need to build the model and also a docker file which will contain all the path information of the inputs required for the model and also install all the dependency softwares required to run the model in the proper environment.
