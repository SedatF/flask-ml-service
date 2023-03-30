[![Python application test with Github Actions](https://github.com/Abdulrazak-Alahmad/flask-project/actions/workflows/main.yml/badge.svg)](https://github.com/Abdulrazak-Alahmad/flask-project/actions/workflows/main.yml)

# Azure DevOps Project: Building a CI/CD Pipeline

# Overview

This project describe the DevOps CI/CD concepts. using Azure pipeline and Github Actions for automating test, build and deploy web application. 

## Project Plan


* The [Trello board](https://trello.com/invite/b/3YU07DJm/ATTIda6372467fcfd690bb81ed1363c14b3bB815D282/weekly-plan) is then used for task planning and tracking.
* The [quarterly project plan](../../raw/main/plan/CI-CD_yearly_quarterly_project_plan.xlsx) the steps for building CI-CD pipeline.

## Instructions
The below diagram shows the project architecture.  

![project architecture](screenshot/Architecture_of_CI_CD_Project.png "project architecture")

The source code are in GitHub repo, actually GitHub Actions perform CI. therefore once any change happend on repo the GitHub Actions can atomatically check the code by build and test.

The code was cloned, build and deployed locally to Azure app serive.

Azure pipline perform CI/CD by pulling the code from GitHub, Build, Test and Deploy it to Azure app service.

### Cloning GitHub Repo and Testing Locally

open the Azure cloud shell by using your credential.

Clone project from GitHub and change to the project directory:
```bash
odl_user [~]$ git clone git@github.com:Abdulrazak-Alahmad/flask-project.git
odl_user [~]$ cd flask-project
```

Create python virtual env & source :
```bash
odl_user [~/flask-project]$ python3 -m venv ~/.myrepo
odl_user [~/flask-project]$ source ~/.myrepo/bin/activate
```
![ GitHub Clone Repo](screenshot/github_clone.jpeg "Clone repo / GitHub Clone Repo")

Install needed packages and testing it:
```bash
(.myrepo) odl_user [~/flask-project]$ make all
```
![Build project](screenshot/make_all.jpeg "Build project")

Run the application locally:
```bash
(.myrepo) odl_user [~/flask-project]$ flask run
```

Test the code locally in new Azure Bash:
```bash
odl_user [~]$ source ~/.myrepo/bin/activate
(.myrepo) odl_user [~]$ cd flask-project/
(.myrepo) odl_user [~/flask-project]$ ./make_prediction.sh
```

![Test locally](screenshot/prediction.jpeg "Test locally")

### Provisioning CI using Github Actions
Performe CI by using GitHub Action.

From the top bar of GitHub click on 'Actions', then click on "set up a workflow yourself' and use the GitHub Actions template yaml file located in  [.github/workflows/main.yml]

Once you create this workflow, it will run automatically to build code in Repo:
![GitHub Actions](screenshot/run_action.jpeg "GitHub Actions")

Passing GitHub Actions:
![GitHub Actions](screenshot/passed_actions.jpeg "GitHub Actions")

### Deploying to Azure App Services
Deploy app to Azure app services locally using Azure CLI:
```bash
(.myrepo) odl_user [~/flask-project]$ az webapp up -n flask-abdulrazak --sku F1 --resource-group Azuredevops
```

Check app if it is become online by using the link from the previous step:

![check webapp](screenshot/Azure_running_webapp.jpeg "check webapp")

Test the online app by invoke 'make_predict_azure_app.sh'  modify webapp name in the file
Edit file 'make_predict_azure_app.sh' and replace '< yourappname >' with your webapp name (e.g. flask-abdulrazak).

Test the remote webapp:
```bash
(.myrepo) odl_user [~/flask-project]$  ./make_predict_azure_app.sh
```
![Test remotely](screenshot/remote_prediction.jpeg "Test remotely")

Logs of webapp can be easily done by tail linux command:

open cloud shell 

```bash
(.myrepo) odl_user [~/flask-project]$ az webapp log tail
```

![Log](screenshot/log.jpeg "Log")

validation of the webapp can be performed using [locust](https://locust.io).

Install locust tool 

(.myrepo) odl_user [~/flask-project]$ pip install locust

![Install locust tool](screenshot/locust_install.jpeg "Install locust tool")

Open Template file 'locustinput.py' and Replace '< yourappname >':
```bash
(.myrepo) odl_user [~/flask-project]$ nano locustinput.py
(.myrepo) odl_user [~/flask-project]$ locust -f locustinput.py --headless -u 10 -r 3 -t 10s
```

![locust_test](screenshot/locust_log.jpeg "locust_test")

### Provisioning CI/CD using Azure Pipelines

This pipeline will get the code from GitHub Repo and do all operations building, testing and deployment.

Go to Azure devops from your Azure account  https://dev.azure.com.

Create a New Project.

Click on 'New pipeline' from the left panel.

Link your GitHub Repo to pipeline

Configure pipeline to deploy code to Azure app service 'that created in previous stage' by providing suitable inputs according to your Azure subscribtion

run the pipeline including the 'Build stage' and the 'Deploy Web App' based on yaml file:

![Azure_pipeline_build_deploy](screenshot/Azure_pipeline_build_deploy.jpeg "Azure_pipeline_build_deploy")

View pipeline log by click on build icon

![Azure_pipeline_build_deploy_log](screenshot/Azure_pipeline_build_deploy_log.jpeg "Azure_pipeline_build_deploy_log")

From now on every change to your code will trigger the CI/CD pipeline and update your webapp accordingly:

Change the application name in app.py from 'Sklearn Prediction Home' to 'Sklearn Prediction Home via Azure CI/CD Pipeline' and commit it:
```bash
(.myrepo) odl_user [~/flask-project]$ nano app.py
(.myrepo) odl_user [~/flask-project]$ git add app.py && git commit -m "Change app name" && git push
```
![change_appname_and_push](screenshot/change_appname_and_push.jpeg "change_appname_and_push")
App name before changing:
![appname_before_change](screenshot/appname_before_change.jpeg "appname_before_change")
App name after changing:
![appname_changed](screenshot/appname_changed.jpeg "appname_changed")

The pipeline is triggered by each commit to GitHub Repo and actually that is the CI/CD
![pipeline_triggered1](screenshot/pipeline_triggered1.jpeg "pipeline_triggered1")
![pipeline_triggered2](screenshot/pipeline_triggered2.jpeg "pipeline_triggered2")
![pipeline_triggered3](screenshot/pipeline_triggered3.jpeg "pipeline_triggered3")
![pipeline_triggered4](screenshot/pipeline_triggered4.jpeg "pipeline_triggered4")


## Enhancements
Future improvements include but are not limited to:
* preform automatically testing using testing module such as locust.


## Demo

This video demonstrates all previous steps:
[Demo Video](https://www.youtube.com/watch?v=7WVkz0Brn0E)

