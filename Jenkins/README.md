Jenkins
```	What are the Jenkins shared libraries and how do they work? ```
As a DevOps engineer, I work with multiple development teams. For each team I create CI/CD pipelines, there are some pieces which are common across the development teams
-static code analysis
-build stage and the code we use also same as all the development teams use java
- deployment logic is also similar
For these we started using Jenkins shared libraries. Because Jenkins is our CI/CD platform
When ever we create new ci/cd pipeline we will check the Jenkins shared library can be used instead of using the ci/cd pipeline from zero if yes we will invoke that in the Jenkins file. And if any code that is not available in the Jenkins library we will write that code and we use github for storing the shared libraries 
<img width="975" height="326" alt="image" src="https://github.com/user-attachments/assets/12247a79-d93a-4d12-8751-87891c9ac01b" />
 

```	Talk about 5 build targets that you use in day-to-day tasks? ```
As a devops engineer I will work on Ci/cd and build process as part of that I will use the build targets for example developer gives me a source code java source code and the build language is maven
The build targets I will do are 
 compile the source code
running the unit test
cleaning the previous builds so the current build is not impacted 
creating Packages like jar, war
installing the created package on to local repo or external repository like jforg  

mvn clean – clean the builds previous builds in / target directory – target directory holds all the previously created jar, war file artifacts before running the new build it is important to clean the target directory
mvn complie—need to complie java files with in src/main/java directory
mvn test – to run the unit test cases as a part of the unit testing when I run mvn test /src/test/java files are executed
we need to package the files into jar or war files we run the command
mvn package—where the mvn package creates the actual target artifact
mvn install – for placing the artifacts in to local repo or or remote repository by default it will place the artifacts into ~/.m2 directory which is default maven repository
I can also pass some parameters to mvn install so that I can push the artifcats to jfog or nexus.

```	Which artifact repository do you use for builds```
In the current organization we use nexus or jfrog as the current artifact repository
Antifactory is used for downloading the third party packages only supported versions of third party dependencies are placed in antifactory repository
Apart from that when we  create build we place jar file, war files and entrerprise files into the artifact repository.
 <img width="659" height="228" alt="image" src="https://github.com/user-attachments/assets/82df95b5-940e-4f31-a6be-d7beba1c8cfb" />


```	How do you configure Artifactory for your application in Maven? ```
I will configure the settings.xml file a default settings file with in that we can configure the location for the artifactory in our case, we use a self-hosted Nexus. We use nexus as artifactory and this nexus is self hosted on on-premises or aws infrastructure so we will provide the location for nexus within the settings.xml files

We can also provide the location for nexus in application as pom.xml file
In pom.xml we can configure distributive management block  and provide the location for the artifactory
 <img width="897" height="173" alt="image" src="https://github.com/user-attachments/assets/8ac61270-1454-4a6a-a63e-9ef3decf95b5" />


```	Builds pass locally but fail in CI, How do you troubleshoot it? ```
1.As a first step I will go through the build log and search for any reasons for build failure
2.as a second step I will look into the dependency caching, I will check the developer ran mvn clean before running the mvn package. It might be possible while developer working from local build the builds were picked up from the target directory so it can be issue with dependency caching
3. if the issue is not with the dependency caching I will look for the environment variables, it is possible that developer might be set some env variables locally, when running the ci/cd pipeline those environment variables are not properly set
4. missing file permissions when running the ci/cd pipelines the Jenkins agent might not have the required permissions
5. compare developers local env with ci/cd env suppose local is windows and ci/cd is linux  or issue with operation system or distribution
 <img width="498" height="302" alt="image" src="https://github.com/user-attachments/assets/56ed92ed-3c1c-4771-99fd-c4d263cbdab3" />


```	CI pipeline succeeds but app is broken in prod? ```
We have dev stage and prod environments and it broke in production
We will check what are the difference in the environments where the stage and production are same stage is having 3 node cluster and production is having 5 nodes cluster 
1.	Traffic in production  where in stage you have tested with 1000 users but in real time it is 10000 or during peak hours 100000 users will check the logs of the application describe the pod and will check the reason for the application failure, if any crash loop back off will check why this crash loop backoff occurred if it is because of OOM Killed then it is traffic.
For troubleshooting I will inform to perform the load testing in the and performance testing   in tagging environment where we can simulate similar traffic. So we can identify 
If the issue is not with the traffic QA team already performed the load and performance testing then issue is not with traffic 
2.deployment strategy: will go with canary deployment instead 100% deployment of the version to production we will deploy 10% of the new version of the application first it has only 10% of the traffic and after 2 or 3 days will increase to 30% of new version then gradually increase to 50% and then 100%
3. Any differences in the ci/cd build of stage and prod between two environments  compare both 


```	Pipelines slow down over the time? ```
As the devops pipeline taking more time compared to previous time and increasing more and steps to be followed to decrease the time
1.Parallelism if we are using Jenkins as a CI/CD tool. If we are using single node or agent all the builds goes to the same agent, on the agent if you configured concurrence or parallelism  as 2 the only 2 pipelines are executed successfully. If that agent has multiple cpu we can configure the concurrency as 4 in that case 4 ci/cd pipelines can run at at time 
Suppose 4 concurrency in place and 16 ci/cd pipelines are executing then only after the 4 pipelines are executed then only the agent will be set to free. So if the pipeline takes 30 min time to run and to get free up time of the agent it will take more time.
 <img width="764" height="205" alt="image" src="https://github.com/user-attachments/assets/08f3e45d-3fde-4ef3-9377-794a7b75c051" />

2.	To get the build time down we have BUILD CACHE when there are common things in the pipeline, when we run ci/cd pipeline same steps docker related steps are executed, if there are no changes in the build stage it will be cached from previous build layer, if now changes in the docker image creation in the pipeline.

```	A developer pushes a feature branch, but pipeline does not trigger```
Where all the branches are able to trigger the CI, but not the feature branch. So go to the YAML file of the GitHub Actions, and there you'll find one parameter as on  and check whether push field is configured or not and I will try to see any excludes added to the push field
 <img width="764" height="205" alt="image" src="https://github.com/user-attachments/assets/08122d5a-1845-41ec-b7c9-b06af735bca7" />

```	Your build failed because it cannot download the dependency from your artifact repository? ```
Java as programing language
Maven as build tool
Nexus as antifactory 
First we need to check whether the dependency exists in artifactory repository or not  
Will login to nexus and go to the location in pom.xml and version of the dependency is available or not 
Suppose developer is looking for particular version if that is not available will push the version from next time build will success if that version exists
Check setting.xml file where settings.xml is a parent file for maven repository or main configuration of maven with in ~/.m2 folder will see whether nexus is configured right or not
All the applications pickup nexus configuration from settings.xml file if required credentials will also check the credentials rightly configured or not 
I will check Nexus by logging into Nexus. The pom.xml the path in the xml for dependency i will check the right version of the artifact is present or not if not available after taking the approval will pus the right version

```	Python build fails in CI but works locally. What could be the issue?? ```
For this question, it needs to be in a troubleshooting way.
1.	We need to go to the build CI log which is GitHub Actions, Jenkins, or GitLab. We need to check the build failure log. And look into if there is any straightforward issues like timeout, Lack of resources on the CI, for this we can got to the CI increase the timeout or resources
2.	Most common with the Python builds is lack of virtual environment. When they are running locally, they might not have used the virtual environment. A virtual environment in python is a isolation environment When there are two applications running, a virtual environment is definitely used because not mixing of the dependencies of both applications. Developer is not using the virtual environment; request user to rebuild the application by using virtual environment.
3.	Comparing the environment developers local environment and environment on the CI where developer is using windows operating system and CI is linux, I'll try to see the parameter set by the developer in Windows, and the same parameters were reflected on the linux
4.	Caching issue in local  Developer might not have cleared the cache of previous Build artifactory we will ask developer to clear the cache and run Because in CI every time a new virtual machine is created and a new build is run it is always a fresh environment.

```	Explain the Python application build process in detail? ```
   <img width="264" height="244" alt="image" src="https://github.com/user-attachments/assets/9c2d1a03-a041-4b9f-8aa9-e73e368547e4" />
<img width="581" height="441" alt="image" src="https://github.com/user-attachments/assets/021e71eb-8b7f-4ddb-8b7d-4bb2977a5f2d" />

example of the application structure
First, we will define that pyproject.toml file is the same as pom.xml file where it will have the details of the project like Name Version other details Exactly how you're defining pom.xml The building -system is mandatory.
 <img width="719" height="120" alt="image" src="https://github.com/user-attachments/assets/79ee6980-82cf-41c7-86a3-42c60b30bb00" />

We will run 
Pip install build    once build id successful 
Python3 -m build when we run this a dist directory is generated  and with in the dist directory we will create .whl extension file same as jar file in java some time zip file also 
Depends on the aritifact we use 
Pip install twine   --using twin we push the artifact to PyPI which is same like Nexus
twine upload dist/*   -- we will upload everything in the .whl or zip file 

```	Using static code analysis what kind of problem we can identify? ```
Static code analysis tool is sonarqube
1.they capture syntax errors
2. identify unused variables or functions
3. style violations (indentation )
4. Type mismatch
5.basic Security issues

```	Static code analysis slowdown CI process, how will you fix it? ```
Initially the application Is 100 lines later it is 1000 lines and now it Is 20000 lines so it will be slowing down the process I followed these two steps to decrease the time.
1.to run static code analysis only on the changed files  for every commit or every pull request there will be certain number of changed files 
To implement this I will write git diff command and identify what files are changed in the Pull request and if we use python as programming language and flake as static code analysis we will run flake 8 $ build files 
2.run the full proof static code analysis only on the nightly builds—every organization runs ci/cd on every night once developer commits all the changes after all the developers are committed the changes a CI pipeline is run to ensure the code in the main branch is fully stable this when developer commits or pull requests I will run on the changed files 

```	App in “outofsync” state in Argo CD, But no git changes? ```
In the gitOps approach and deploying the application into Kubernetes 
Basically Kubernetes manifests placed in git repository all the deployment Kubernetes files service manifests everything is placed in git repository and we use argocd for deploying those artifacts on to the required namespace in the Kubernetes cluster while this is complete automated process then why the application is outofsync the following reasons could be 
1.manual changes made to the application in the Kubernetes cluster directly by the users in gitops approach we should not deploy anything manually to the Kubernetes cluster if this is the case I will click the sync button in argocd it will automatically replace the new version with old version in the git or if we enable the auto synch then it will automatically sync 
2.deleetd the resource – sync button argocd will redeploy
3.
3. if we don’t know what changes made in the argocd  command line if we run argocd app diff, this will show me the exact reason why the application is showing as outofsync

```	 When a build fails in Jenkins, how will you send an email? ```
We need to send email notification only when the build fails and second one is it free style or declarative pipline
For free style—we should install email extension plugin  and head to the Jenkins job, go to configurations of the Jenkins job with in post build  option look for editable email notification and with in the editable email notification select the trigger as failure.
 <img width="413" height="322" alt="image" src="https://github.com/user-attachments/assets/73bc5593-de49-4cb7-bfdd-a7039390c26e" />
 Declarative—with in the Jenkins file that is used to trigger the action need to write a post action or post section{ failure{ incase of failure send the mail
 <img width="975" height="459" alt="image" src="https://github.com/user-attachments/assets/e4c855f5-8a22-4125-b649-d75f19aa31b9" />






Declarative—with in the Jenkins file that is used to trigger the action need to write a post action or post section{ failure{ incase of failure send the mail
