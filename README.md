# Docker-Container-and-Application-using-Docker-Compose

Run a Docker Container and a Docker python Application using Docker-Compose and what ever front-end language .The result will be Docker Engine and Docker-compose running and a docker application.

First of all , what is The docker in simple words, is tool that lets you to put the application into a container. these containers allow you to package up the application with all of the parts it needs, so, all libraries and the Independence can deploy in one package .

#### so, lets start dockerizing our python application :

try to install Docker Desktop by going to  [official site](https://www.docker.com/get-started/) and click on get started at the top on right corner of the page.so, choose your operating system and follow the installation guide. 


> ### @icon-info-circle  Desktop Version
> Note : I recommended desktop version for reason, it give you both the command line interface and useful desktop application.

one you downloaded this ,you need to verify this in your terminal.

#### verify current docker version  

    docker -v
    
this should tell you the current version , so you can see that you have it now on your machine. 

-----------

> ###  The Editor 
> Note : I use the visual studio as editor , but you can use the Git batch at (windows) and install python on it to write the code files on it.

#### Docker Extension 

you need to install this in visual studio code first by **Press the (CTRL + SHIFT+ P ) to open the command palette** then type **Install Extensions** and **click on** , and in the bar search for **Docker** that from **Microsoft ** this give nice features for example, debugging or for auto completion   .  

so lets start dockerlizing 


#### For creating the virtual environment use this terminal .

    python -m venv venv 
    
**For activating this from scripts** , for reason On Windows, virtualenv creates a .bat/.ps1 file, so you should using this command :
    
     venv\Scripts\activate
     
     
 now, the environment created , so you needto install the Independence :
 
     pip install fastapi
     
     pip install uvicorn
     

#### Writing the python Script

I want to have the single python script 

I go to **File** --> open folder --> **create folder** in you path location then **open it** after that, **create file** name it as **"main.py"** . This The created script should be **(inside the app folder)** is the following:

here create files, and define the app and function for app 
by **from fastapi import FastAPI** , the difine the route ** @app.get("/")** , so have one route with slash item ** @app.get("/items/{item_id}")**
    

    from typing import Optional
    
    from fastapi import FastAPI
    
    app = FastAPI()
    
    
    @app.get("/")
    def read_root():
        return {"Hello": "World"}
    
    
    @app.get("/items/{item_id}")
    def read_item(item_id: int, q: Optional[str] = None):
        return {"item_id": item_id, "q": q}
       

so, this code using user input .

before go to write docker file , you should differentiate between 3 things such as Docker file , Image, Container. 

**Docker file** : is blueprint for building image.

**Image:** is template for running containers .

**Containers** : is the actual running process where you have the package project .

-------

#### now lets test our app locally by this command :

     uvicorn main:app --reload
     
#### or by second way :

    import unvicorn 
    
    if __name__== '__main__':
    
          uvicorn.run(app )
          
  so, install uv labraries and run the app by terminal 

----------

#### now, you should run the app 
write this command in terminal :

        cd app
        
 so, to go to the directory of the app 
 then lets say 
 
       python main.py 
 
 it say the server running and can go the url by going to localhost 8000
 

you will get the hello world response like this :

![](../Documents/GitHub/Docker-Container-and--Application-using-Docker-Compose-/Screenshot%202022-05-12%20152203.png)


then, can shut down the server by **CTRL+C**

-------
to save all Independence and requirements in container , put this in terminal :

    pip freeze > requirements.txt
    
This writing the dependencies into this file should be in root directory .




#### Starting the Docker-file

first things , should define the images , and specify the python version. this is pulling image in docker hub  which has python already installed .then , you must add main.py inside the container and add (dot .) to destination that you have it . i use the request  .also, creating work directory by using **WORKDIR** and call the directory in the container , copy the requirements and specify the destination of this file and run the requirements.txt . finally, copy the app folder on this directory on the machine , then specify the directory on the container .After that , to check if the container run , and main.py inside our container , I put the CMD.  

This code for dockerfile :

        # For more information, please refer to https://aka.ms/vscode-docker-python
        FROM python:3.8-slim
        
        
        # Install pip requirements
        COPY requirements.txt .
        RUN pip install gunicorn
        RUN python -m pip install -r requirements.txt
        
        WORKDIR /app
        COPY . /app
        
        EXPOSE 8000
        
        # During debugging, this entry point will be overridden. For more information, 
        please refer to https://aka.ms/vscode-docker-python-debug
        CMD ["gunicorn", "--bind", "0.0.0.0:8000",  "main:app"]
        
        #gunicorn --bind 0.0.0.0:8000 main:app

now , we need to build our docker image again by putting this command on the terminal :

    docker build -t python-fastapi
    
  I need now to put the location, so just add the (dot .) :
  
      docker build -t python-fastapi .
      
 
 now image build and can you see executing for this in this order 
 
![ ![the output for building image ]((file:///C:/Users/Shahad%20Alzughaibi/OneDrive%20-%20Deer%20ITC/Documents/GitHub/Docker-Container-and--Application-using-Docker-Compose-/Screenshot%202022-05-12%20143102.png](../Documents/GitHub/Docker-Container-and--Application-using-Docker-Compose-/Screenshot%202022-05-12%20143102.png)




      
-----------

#### For user input code that I putted on main.py 


> ###  user input 
> Note : It must use additional arguments , to run in docker by this commend , also, use -t , use for giving the pseudo terminal, and where can type in user input . In addition, the -i , to be in interactive mood 

the command is to run in terminal :

     docker run -t -i python-imbd

so , it will ask me if I want to see a movie , said "y" , and if not want to see the movie said "n"    

-----------------

#### now, should build image again .

now , we need to build our docker image again by putting this command on the terminal :

    docker build -t python-fastapi
    
  I need now to put the location, so just add the (dot .) :
  
      docker build -t python-fastapi .
      

-----

 ###  to running and start the container , use this command :
   
      docker run python-fastapi
      
 -----
 
### now you need to specify the port and host to get to server .

so, write this code on main.py :


      uvicorn.run(app, port=8000, host="0.0.0.0" )

and should build container again so, follow the 2 steps above for build and putting location 

## then docker run with port 

by putting the this command on terminal :

    docker run -p 8000:8000 python-fastapi
    
   In browser , put localhost:8000 . the web application work. 
    
    
if you put the localhost:8000/items/9
this work also.so, all working now .

-----

### for Docker Compose :

go to you need to docker compose file this in visual studio code , so first  **Press the (CTRL + SHIFT+ P ) to open the command palette** then type **Docker: Add Docker Compose Files to Workspace ** and **click on** , and in the bar search for **Docker** select the application platform that is  the that from **python:FastApi ** then ** the path has virtual machine and main.py  ** . now , the file has docker compose  . 


-----
#### From CMD in your machine (Windows)

    docker-compose build .
    
 to build the docker compose
 
 ----
 
###  For try to start the docker (container) in the background 
 
 for aggregates the output of each container. it is running docker compose up to start the containers in the background and leaves them running.
 
this command :

    docker-compose up
 
 
 
 



     
    
    
    
    





      
      
  
  
    
    
    
    


















