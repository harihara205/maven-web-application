node{
    def mavenHome = tool name: "maven3.8.6"
     echo "The JobName is: ${env.JOB_NAME}" 
     echo "The node name is: ${env.NODE_NAME}"
     echo "The build number is: ${env.BUILD_NUMBER}"
     properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: '']])
     buildName 'Dev - ${ BUILD_NAME}'
    //checkout stage
    stage('checkout code'){
    git credentialsId: 'f6e348b0-3f7d-4d97-95e0-22830d2e3616', url: 'https://github.com/harihara205/maven-web-application.git'    
    }
    //build stage
    stage('Build'){
    sh "$mavenHome/bin/mvn clean package"    
    }
    //Genarate sonarqube report
    stage('SonarQube report'){
    sh "$mavenHome/bin/mvn sonar:sonar"    
    }
    //upload Artifact into Artifact Repository
    stage('upload Artifact Repo into Nexus'){
    sh "$mavenHome/bin/mvn deploy"
        
    }
    //deploy application into tomcat server
    stage('Deploy application into Tomcat'){ sshagent(['d4d56a8b-4506-4502-8c0f-bc598db1f4bc']) {
    
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.235.68.73:/opt/apache-tomcat-9.0.68/webapps"
    
}
        
        
    }

    
}
