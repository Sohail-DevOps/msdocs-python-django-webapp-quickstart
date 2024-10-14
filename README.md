# Deploy a Python (Django) web app to Azure App Service - Sample Application

This is the sample Django application for the Azure Quickstart [Deploy a Python (Django or Flask) web app to Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/quickstart-python).  For instructions on how to create the Azure resources and deploy the application to Azure, refer to the Quickstart article.

Sample applications are available for the other frameworks here:

* Flask [https://github.com/Azure-Samples/msdocs-python-flask-webapp-quickstart](https://github.com/Azure-Samples/msdocs-python-flask-webapp-quickstart)
* FastAPI [https://github.com/Azure-Samples/msdocs-python-fastapi-webapp-quickstart](https://github.com/Azure-Samples/msdocs-python-fastapi-webapp-quickstart)

If you need an Azure account, you can [create one for free](https://azure.microsoft.com/en-us/free/).

## For local development

Fill in a secret value in the `.env` file.

For local development, use this random string as an appropriate value:

```shell
SECRET_KEY=123abc
```

## When you deploy to Azure

For deployment to production, create an app setting, `SECRET_KEY`. Use this command to generate an appropriate value:

```shell
python -c 'import secrets; print(secrets.token_hex())'
```





===========================================Here is the Step by Step Guide for Django Application Deployment on Azure App Service===================
Django application
git clone https://github.com/Azure-Samples/msdocs-python-django-webapp-quickstart
Go to the application folder:
cd msdocs-python-django-webapp-quickstart
Create a virtual environment for the app for windows:
py -m venv .venv
.venv\scripts\activate
Create a virtual environment for the app for macOS/Linux:
python3 -m venv .venv
source .venv/bin/activate
Install the dependencies:
pip install -r requirements.txt
Run the app:
python manage.py runserver
Browse to the sample application at http://localhost:8000 in a web browser.
Create a web app in Azure

1.	Enter app services in the search bar at the top of the Azure portal.
2.	Select the item labeled App Services under the Services heading on the menu that appears below the search bar.
3.	On the App Services page, select + Create, then select + Web App from the drop-down menu.
4.	On the Create Web App page, fill out the form as follows.
1.	Resource Group → Select Create new and use a name of msdocs-python-webapp-quickstart.
2.	Name → msdocs-python-webapp-quickstart-XYZ where XYZ is any three random characters. This name must be unique across Azure.
3.	Runtime stack → Python 3.9.
4.	Region → Any Azure region near you.
5.	App Service Plan → Under Pricing plan, select Explore pricing plans to select a different App Service plan.
The App Service plan controls the amount of resources (CPU/memory) that are available to your app and the cost of those resources.

For this example, under Dev/Test, select the Basic B1 plan. The Basic B1 plan will incur a small charge against your Azure account but is recommended for better performance over the Free F1 plan.
When finished, select Select to apply your changes.
On the main Create Web App page, select the Review + create at the bottom of the screen.

This will take you to the Review page. Select Create to create your App Service.

Deploy Your Code to Azure App Service
1.	Set Up Local Git Deployment:
o	In the Azure portal, go to your App Service.
o	In the left menu, find "Deployment Center" under Settings.
o	Select "Local Git" as the source.
o	Click on "Continue" and then "Finish". Note the Git remote URL provided by Azure.
Or you can use GitHub by the given instructions at Azure Portal


===================
===================
Workflow
===================
===================
# Docs for the Azure Web Apps Deploy action: https://github.com/Azure/webapps-deploy
# More GitHub Actions for Azure: https://github.com/Azure/actions
# More info on Python, GitHub Actions, and Azure App Service: https://aka.ms/python-webapps-actions

name: Build and deploy Python app to Azure Web App - msdocs-python-webapp-quickstart123

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Set up Python version
        uses: actions/setup-python@v5
        with:
          python-version: '3.9'

      - name: Create and start virtual environment
        run: |
          python -m venv venv
          source venv/bin/activate
      
      - name: Install dependencies
        run: pip install -r requirements.txt
        
      # Optional: Add step to run tests here (PyTest, Django test suites, etc.)

      - name: Zip artifact for deployment
        run: zip release.zip ./* -r

      - name: Upload artifact for deployment jobs
        uses: actions/upload-artifact@v4
        with:
          name: python-app
          path: |
            release.zip
            !venv/

  deploy:
    runs-on: ubuntu-latest
    needs: build
    environment:
      name: 'Production'
      url: ${{ steps.deploy-to-webapp.outputs.webapp-url }}
    permissions:
      id-token: write #This is required for requesting the JWT

    steps:
      - name: Download artifact from build job
        uses: actions/download-artifact@v4
        with:
          name: python-app

      - name: Unzip artifact for deployment
        run: unzip release.zip

      
      - name: Login to Azure
        uses: azure/login@v2
        with:
          client-id: ${{ xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx }}
          tenant-id: ${{xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx }}
          subscription-id: ${{xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx }}

      - name: 'Deploy to Azure Web App'
        uses: azure/webapps-deploy@v3
        id: deploy-to-webapp
        with:
          app-name: 'msdocs-python-webapp-quickstart123'
          slot-name: 'Production'
          



