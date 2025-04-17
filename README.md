# Parts-Unlimted .Net WebApp E2E

- [Project Description](#ProjectDescription)
- [Services used in the project](#Servicesusedintheproject)
- [E2E walk-through: CI](#e2e-walk-through-ci)
	- [Dev environment:](#dev-environment)
	- [Creating the DB connection for each environment :](#creating-the-db-connection-for-each-environment)
	- [Starting the Parts Unlimted CD and assigning where is the Artifact :](#starting-the-parts-unlimted-cd-and-assigning-where-is-the-artifact)
- [E2E walk-through: CD](#e2e-walk-through-cd)
	- [A glimpse into the deployment stage work in progress.](#Aglimpseintothedeploymentstageworkinprogress.)
	- [We Have successfully deployed our website into the cloud provider.](#WeHavescuessfulydeployedourwebsiteintothecloudprovider)
	- [preparing the other deployment stages and conditional approval for the production environment.](#Preparingtheotherdeploymentstagesandconditionalapprovalforproductionenvironment.)
	- [We can see that our dev-env and qa-env already live and the prod still waiting for my approval on the cd to prevent Mistakes , errors and bugs from reaching the main production environment.](#Wecanseethatourdev-envandqa-envalreadyliveandtheprodstillwaitingformyapprovalonthecdtopreventMistakeserrorsandbugsfromreachingthemainproductionenvironment.)
	- [I navigated back into Azure DevOps platform and give the approval for deployment.](#InavigatedbackintoAzureDevOpsplatformandgivetheapprovalfordeployment.)
	- [We Can see after clicking the approvel we can see that we our final stage already launched and live.](#WeCanseeafterclickingtheapprovelwecanseethatweourfinalstagealreadylaunchedandlive.)
- [Deployment using terraform.](#Deploymentusingterraform.)
- [Iac CI](#IacCI)
- [Iac CD](#IacCD)
- [To-Do](#To-Do)
	- [Containerized the webapp for easier deployment.](#Containerizedthewebappforeasierdeployment.)

##  <a name='ProjectDescription'></a>Project Description
The Project is a simple website that provides the ability to buy and sell parts as an e-commerce website I have decided to create the full cycle of DevOps CI/CD Life Cycle and I will go and describe what I'm doing in each phase so let's start.


##   <a name='Servicesusedintheproject'></a>Services used in the project

- Azure DevOps repos,CI/CD Pipelines/Artifact.
- Azure Cloud WebAppServices / Azure SQL.
- Iac deployment with Terraform.

##   <a name='E2Ewalk-through:CI'></a>E2E walk-through: CI 

- I have used Azure Repos to upload the source code of the used example.
- I have configured a CI Pipeline to automatically build upon any new commit to the azure repo You can Check up the link for the CI [Here](https://dev.azure.com/DevOps-v-tutorial/Devops-course-tutorial/_build/results?buildId=26&view=logs&j=275f1d19-1bd8-5591-b06b-07d489ea915a&t=40b1ee41-44d6-5bba-aa04-4b76a5c732e5) the following image shows up the used sequence on the CI Pipeline, After the CI is done with the build will store the package on Azure drop artifact.

CI Pipeline build agent sequence:

![](assets/images/01.Ci.jpg)

- After that, it was the Azure server backend creation of Resource group that contain Web-app - Database and server for the connection.

- To create the full deployment plan I have created an environment for each stage Dev,QA and deployment the following pics show up creation of each stage.

- This is Dev environment  example and i don't want to post so many pictures so i will show only the dev env for now and you can check the others in the [Resources](assets/images/)
  
###   <a name='Devenvironment:'></a>Dev environment: 
  
![](assets/images/01.RG-Dev.jpg)


###   <a name='CreatingtheDBconnectionforeachenvironment:'></a>Creating the DB connection for each environment :

![](assets/images/08.connecting%20db.jpg)

###   <a name='StartingthePartsUnlimtedCDandassigningwhereistheArtifact:'></a>Starting the Parts Unlimted CD and assigning where is the Artifact :
![](assets/images/09.Publish.jpg)

##   <a name='E2Ewalk-through:CD'></a>E2E walk-through: CD 

-You can check the CD on Azure DevOps from [Here](https://dev.azure.com/DevOps-v-tutorial/Devops-course-tutorial/_release?_a=releases&view=mine&definitionId=3)


###   <a name='Aglimpseintothedeploymentstageworkinprogress.'></a>A glimpse into the deployment stage work in progress.
![](assets/images/010.deployment%20is%20happening.jpg)

### <a name='WeHavescuessfulydeployedourwebsiteintothecloudprovider'></a>We Have successfully deployed our website into the cloud provider
![](assets/images/014.Working%20website.jpg)


###   <a name='Preparingtheotherdeploymentstagesandconditionalapprovalforproductionenvironment.'></a>Preparing the other deployment stages and conditional approval for production environment.
![](assets/images/015.aprroval.jpg)


###   <a name='Wecanseethatourdev-envandqa-envalreadyliveandtheprodstillwaitingformyapprovalonthecdtopreventMistakeserrorsandbugsfromreachingthemainproductionenvironment.'></a>We can see that our dev-env and qa-env already live and the prod still waiting for my approval on the cd to prevent Mistakes , errors and bugs from reaching the main production environment.
![](assets/images/016.waiting%20for%20approvel.jpg)

###   <a name='InavigatedbackintoAzureDevOpsplatformandgivetheapprovalfordeployment.'></a>I navigated back into Azure DevOps platform and give the approval for deployment.
![](assets/images/017.aprroval%20stage.jpg)
  
###  <a name='WeCanseeafterclickingtheapprovelwecanseethatweourfinalstagealreadylaunchedandlive.'></a>We Can see after clicking the approval we can see that we our final stage already launched and live.
![](assets/images/018.After%20aprroving%20all%203%20working.jpg)


##   <a name='Deploymentusingterraform.'></a>Deployment using Terraform.

- we will notice the difference between the manual steps of deployment even though both is given in a CI/CD pipeline but the Iac is making it much easier since it will create the Infrastructure and  its components in the background given a single terraform file that will provide us with much easier for preparing multi environments and re-usability.

- You can review the Iac from [Here](infra/websql.tf)

- The terraform file will configure the resource groups, SQL DB, maintain the connection string and configure the firewall rules that we did manually.

##   <a name='IacCI'></a>Iac CI

- The difference in the Ci for the Iac that we will maintains a copy of the tf along to be stored with the publish artifact so it can be used by the CD pipeline as shown in the picture below.

![](assets/images/019.%20Infra-copy-files.jpg)

##   <a name='IacCD'></a>Iac CD 

- We can notice that we have new added tasks to our cd given that we using different deployment methods.
- installing, init then apply the terraform to our agent to be able to perform the terraform tasks since terraform needed to be installed on the machine that performs the given task.
- locate our TF file to be able to deploy it.

![](assets/images/020-TF-int-apply.jpg)

- After the successful run of our CI our deployment to dev env started and as shown in the picture below that we still don't have anything created in our Azure portal.

![](assets/images/021-Azure-empty.jpg)

- After few minutes we can notice that our resource group got created with its components.

![](assets/images/022-Resources%20created.jpg) 

- Our website is live and ready to be used.

![](assets/images/023-scuess.jpg)


##   <a name='To-Do'></a>To-Do
###  <a name='Containerizedthewebappforeasierdeployment.'></a>Containerized the webapp for easier deployment.
