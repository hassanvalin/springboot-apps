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
    stage('Artifactory Configuration') {
      steps {
        script {
          rtMaven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local', server: server
          // rtMaven.resolver releaseRepo: 'libs-release' snapshotRepo: 'libs-snapshot', server: server
          // rtMaven.deployer.artifactDeploymentPatterns.addExclude("pom.xml")
          buildInfo = Artifactory.newBuildInfo()
          buildInfo.retention maxBuilds: 10, maxDays: 7, deleteBuildArtifacts: true
          buildInfo.env.capture = true
        }  
      }  
    }  
  }
}  
  
