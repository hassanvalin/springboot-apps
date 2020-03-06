// def server = Artifactory.server 'Artifactory1'

// def rtMaven = Artifactory.newMavenBuild()

// def buildInfo


pipeline {
  
  environment {
    registry = "hassandocker1/my_java_image"
    registryCredential = 'dockerhub'
    kubeconfig = credentials('azure_kubeconfig')
    dockerImage = ''
  }
  
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
    stage('Build Project') {
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
    /**
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
    stage('Publish build info') {
      steps {
        script {
          server.publishBuildInfo buildInfo
        }  
      }  
    } */
    
    stage('Building image') {
      steps{
        script {
            dockerImage = docker.build(registry + ":$BUILD_NUMBER", "first_spring_boot/")
        }
      }
    }
    stage('Docker Push') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
  }
}  
  
