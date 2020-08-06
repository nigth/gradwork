## Graduation Work
### DevOps_online_Kyiv_2020Q3Q4
___

:heavy_check_mark: **Step 1: GitHub activities**  
Fork Spring Boot example to my new public repository `https://github.com/nigth/spring-boot`:  
https://github.com/spring-projects/spring-boot/tree/2.1.x/spring-boot-samples/spring-boot-sample-web-ui  

:white_check_mark: **Step 2: AWS activities**

Create three EC2 instances with the names `DevTools`, `CI`, `QA`.  
The 4th, `Docker`, EC2 instance will be used only on step number 5.  
Use Amazon Linux AMI 2018.03.0 (HVM), SSD Volume Type on t2.micro instance.  

![ScrShot 01](https://github.com/nigth/gradwork/blob/master/scrshots/01.png "ScrShot 01")  

:black_square_button: **Step 3: Prepare DevTools**  
On the `DevTools` instance install Java 8, Maven, Git, Jenkins and Ansible.  
As an artifact storage solution use Jfrog Artifactory, Nexus or archive artifact inside Jenkins.  

:black_square_button: **Step 4: Configure CI/CD tools**  

Artifactory/Nexus:  
- Create a repository for build artifacts and for deployment procedure
- Create user, which will have access to created repository
_It is possible to archive artifact inside Jenkins instead of using Artifactory/Nexus_

Jenkins:  
- Install required plugins, like git, maven, matrix role
- Disable anonymous access and create some user
- Create build flow with the next steps - CHECKOUT, BUILD, UPLOAD ARTIFACT, CI DEPLOY
- Create deployment jobs for CI and QA environment's
- Steps descriptions:
  - CHECKOUT: should be triggered after each commit to master branch in repository
  - BUILD: to build application use mvn clean install
  - UPLOAD ARTIFACT: upload created artifact with the name <application-name>.<build-version>.jar by Artifactory user
  - CI DEPLOY: Ansible role should deploy latest version of application from Artifactory to CI environment

Ansible:  
- Create deployment role which download and run application. To run application (port should be parametrized):  
```
java –jar path/to/jar/file.jar --server.port=8080 
```
- Create provisioning role, which install Java, Docker, create system user, create folders and other required staff on CI and QA environment's

:black_square_button: **Step 5: BUILD and DEPLOY flow improvement**  

The first, let's containerize app with Docker for the build and deployment flow:  
- Create EC2 instance and install Docker
- Create Ansible build role, which
  - Initiate generation of application.dockerfile which will be used to build application container
  - Build application container
  - Push application container to DTR
- Create Ansible deploy role, which deploy container on Docker node during CI DEPLOY step
- Create Jenkins jobs for deploy of CI and QA environment's with containers
- As option, use AWS ECS service instead of EC2 instance with Docker

Rewrite build and deploy flow to Jenkins declarative pipeline (Jenkins DSL):  
- Build process should use jenkinsfile, which is located in build branch
- Deploy process should use application_deploy.jenkinsfile and this file should be pulled from SCM

Try to use Terraform or AWS Cloud Formation as a IaC approach to create EC2/ECS.  
Add possibility to choose version of artifact / containers during deployment. In simple - it could be common string field. Advanced - drop down menu with artifacts list.  

___

_Thanks for your time!_  





