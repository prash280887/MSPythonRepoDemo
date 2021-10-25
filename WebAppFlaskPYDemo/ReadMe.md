## ******** WebApp Demo using Python and Flask framework & Azure Docker Deployment  ****** ##
Developed By : Prashant Kumar Akhouri
Organisation : Microsoft Corporation
## ********************************************************************************************** ##

# A ) INSTALLATIONS 
## Install Python exe 3.x latest in your System
Install Python 3.x  at https://www.python.org/downloads/ 

## Install Packages and extensions through VSCode -
   Press (Shift+Ctrl+X) in VSCode and install the below extensions - 
    Python   ( and other related extensions )
    Azure App Service  ( for app service creation) 
    Docker  ( for web deployment) 

## Open your project from local or your repository from Github or Azure Repo (ADO) and open in VSCode

1. To load new Code  
   Open VSCode terminal and open WebAppFlaskPyDemo folder 
   cd  WebAppFlaskPyDemo 
    Press   (Ctrl+Shift+P) > type `Python : Select Interpreter`
   It will select and set the installed Python version (displayed at the botton left corner of VScode )

2. To reloading Code from your Azure repository :
   Reload Visual Studio Code once all extensions are installed, you can click the `Reload` button on one of the extensions, or use the `Reload Window` command.
   After reloading run the `Azure: Sign In` command, and follow the instructions to sign into your Azure subscription.
   Select **File > Open Folder**, and navigate to the `WebAppFlaskPyDemo` folder in Azure repository.
   Use the `Python: Select Interpreter` command, and select the `.\env` virtual environment from the list.

## Isolated Installation of Pipenv with Pipx
`Pipx`_ is a tool to help you install and run end-user applications written in Python. It installs applications into an isolated and clean environment on their own. To install pipx, just run:
> pip install --user pipx
Once you have pipx ready on your system (user) , continue to install Pipenv:
> pipx install --user pipenv    
OR
> pipx install --user pipenv 

## Create virtual environment named env in WebApp folder 
Open VSCode Terminal (select Python as language in terminal )
Now create the virtual environment and install required packages.
 > python -m venv env 

## use python from local virtual envrionment env  (activate)
> env\scripts\activate
this command will set terminal to local virtual environment (env) on terminal paths

## Load all WebApplication Preinstaller or requirements (flask,Jinja2,MarkupSafe , Werkzeug etc. ) 
> pip install -r PreRequirementInstalls.txt
 ( Optionally Add  'flask-sqlalchemy' in PreRequirementInstalls.txt  for connectivity to SQL DB
   or run  
   >  pip flask-sqlalchemy

## B) CODE, BUILD and LOCAL RUN 
## BUILD WEB APP  & TEST RUN in DEV 
 Main.py file contains a sample startup code  
 > Right click on main.py file and select "Run Python File in Terminal" 
 OR 
[AppFolderPath]> python main.py
In the Terminal : click the `localhost:5000` default url to goto the default webpage on default Web Browser .

[ NOTE : For first run , incase you get the below error -
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off  * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit) 
   Run the  the below code in terminal  - 
   a) > pip install -r PreRequirementInstalls.txt
      > python .\App\main.py
   Check `localhost:5000` url once again 
 
## C) WEBAPP AZURE DEPLOYMENT : UsING DOCKER  and Azure container registry 
 # Login to Azure in VSCode
  In VSCode Azure Section (Ctrl+Shift+A) > Login to Azure Account & load subsciption  in VS code 
  Once signed in ( Azure account will be displayed on botton left of VSCode)
   
 # Create  Azure Container Registry (ACR)
 Goto Azure portal ,under yoursubscription , Create a azure container registry (ACR , say  webappflaskpy ) in resourcegroup  (say webappflaskpyrg )
 Goto new ACR : webappflaskpy > AccessKeys > (enable Admin user setting) > Copy the Username and Password  
 In VSCode Azure tab - Just check/verify your new ACR : webappflaskpy should be showing in ResourceGroups list under your Subscription 

 # Login to ACR in VSCode Terminal
 > docker login webappflaskpy.azurecr.io 
  [provide login Username and pasword copied from ACR portal ]
  WebAppFlaskPYDemo> docker login webappflaskpy.azurecr.io        
Username: webappflaskpy
Password: <password is from accesskey section in azure prtal ACR - webappflaskpy > 
Login Succeeded

# VSCode Create Dockerfile Image 
  Rifght CLick on Dockerfile > Build Image OR  (Open Ctrl + Shift + P  > Select 'Docket Image : Create Image )  
  Give name of docker image : webappflaskpy.azurecr.io/webappflaskpy:latest 
  Enter ...this will create the docker image
  Refer VSCode Docker Tab > Images section > to check the image  webappflaskpy.azurecr.io/webappflaskpy:latest  created sucessfully 

# Push docker Image to ACR
  Goto VSCode Docker Tab > Images section > open the image  webappflaskpy.azurecr.io/webappflaskpy:latest  created sucessfully 
  Select latest > Right click > Push > keep tag name same as webappflaskpy.azurecr.io/webappflaskpy:latest , press Enter
    Refer VSCode Docker Tab > Registries section (Logged in with Azure Account)  >
   > check under the new ACR :  webappflaskpy > image webappflaskpy:latest will be created 

 # Deploy ACR image to Azure WebApservice 
     Goto VSCode Docker Tab > Registries section (Logged in with Azure Account)  >
   > check under the new ACR :  webappflaskpy > image webappflaskpy:latest will be created 
   Right lick 'latest' > select 'Deploy image to Azure App Service'
   Create the new Azure app service name  (sya webappflaskpydemo ) , then select other setting RG , ServicePlan , Tier etc. and press Enter 
   Refer  VSCode Azure tab - App Service : webappflaskpydemo is created > check its in running state

# Configure  Port in APplication Setting  
    In Docker file : you have nentioned custoem app port as 8000
    Goto VSCode Azure Tab >  App Service > webappflaskpydemo  > ApplicationSettings >  Right click & select Add new Settings 
    > Type WEBSITES_PORT , press ENter 
    > Type 8000 , press Enter 
    Note  Application settings : DOCKER_ENABLE = true  is already there.
## Test the Deployment website  
   Goto https://webappflaskappdemo.azurewebsites.net/ to check the deployed webapp. (will take a while to load)


 