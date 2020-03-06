def server = Artifactory.server 'Artifactory1'

def rtMaven = Artifactory.newMavenBuild()

def buildInfo


pipeline {
  agent any
  
  stages {
    stage('Cloning Git') {
      steps {
        checkout (
        changelog: false, 
        poll: false, 
        scm: [$class: 'GitSCM', branches: [[name: '*/master']],  
        userRemoteConfigs: [[url: 'https://github.com/hassanvalin/springboot-apps.git']]]
        )   }
    }
    stage('Build') {
      steps {
        script {
          sh '''
            cd first_spring_boot
            pwd
            mvn clean install
            '''
        }  
      }  
    }  
  }
}  
  
