# Java-web-application
Java web application repo for CI/CD Pipeline.

In this Project,

=> I used Jenkins as a CI/CD tool by writing a pipeline to fetch the code from github by enabling Webhook trigger to start the build whenever changes made to repo.

=> Configured Maven using jenkins global tool config to run the builds.

=> using the sonar qube plugin from maven to generate the sonarqube report by configuring the sonar server details in jenkins configure system by installing sonar scanner plugin.

=> And configured the Artifactory plugin info in pom.xml to store the build artifact/package in Jfrog artifactory.

=> Configured the deploy to container plugin and given tomcat server details to Deploy the application in tomcat.

=> Now we can access the application from context path of the application or final build name that configured in the pom.xml using tomcat server dns or ip
