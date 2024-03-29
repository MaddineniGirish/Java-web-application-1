node {
    
    def mvnhome = tool name: "maven"
    properties([pipelineTriggers([githubPush()])])
    
    stage ("checkoutcode"){
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/MaddineniGirish/Java-web-application-2.git']]])
    }
    
    stage ("maven"){
        sh "${mvnhome}/bin/mvn clean package"
    }
    
    stage ("Sonar report") {
        withSonarQubeEnv(credentialsId: 'sonarqube') {
          sh "${mvnhome}/bin/mvn sonar:sonar"  
        }
    }
    
    stage ("Artifactory upload"){
        sh "${mvnhome}/bin/mvn deploy"
    }
    
    stage ("Tomcat deploy"){
        deploy adapters: [tomcat8(credentialsId: 'a83025ff-65b6-47e5-9514-2005d6be30b7', path: '', url: 'http://18.189.143.52:8080')], contextPath: null, war: '**/*.war'
    }
    
   post {
        success {
            wrap([$class: 'BuildUser']){
                slackSend(channel: "#general", color: "good",  message: "Status: ${currentBuild.currentResult}, USER: ${BUILD_USER}, Build_ID: #${env.BUILD_ID}, JOB_NAME: ${env.JOB_NAME}, URL: <${env.BUILD_URL}|(Open)>")
            }
        }
        failure {
            wrap([$class: 'BuildUser']){
                slackSend(channel: "#general", color: "#FF0000" , message: "Status: ${currentBuild.currentResult}, USER: ${BUILD_USER}, Build_ID: #${env.BUILD_ID}, JOB_NAME: ${env.JOB_NAME}, URL: <${env.BUILD_URL}|(Open)>")
            }
        }
        unstable {
            wrap([$class: 'BuildUser']){
                slackSend(channel: "#general", color: "#FFBF00" , message: "Status: ${currentBuild.currentResult}, USER: ${BUILD_USER}, Build_ID: #${env.BUILD_ID}, JOB_NAME: ${env.JOB_NAME}, URL: <${env.BUILD_URL}|(Open)>")
            }
        } 
   }
}
