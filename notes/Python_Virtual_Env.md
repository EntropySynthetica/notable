---
tags: [Code]
title: Python_Virtual_Env
created: '2020-01-30T20:16:16.143Z'
modified: '2020-01-30T20:47:52.439Z'
---

# Python Virtual Env
Created Tuesday 18 December 2018


1. Create a folder to store the virtual enviroment with this.  Creates the virtual environment inside the env directory

python3 -m venv env


2. To active the virtual enviroment go into the folder and run

source bin/activate


3. The command prompt should show the name of the virtual enviroment.  The enviroment can be verified with 

which python
and
pip3 list


4. Install any required pip modules.  

pip3 install <whatever>


5. When done put away the virtual enviroment with

deactivate


6. repeat step 2 to return to the enviroment.  



To save your installed dependencies
pip freeze > requirements.txt

To install dependencies you've previously frozen
pip install -r requirements.txt

To see what you have installed
pip list


