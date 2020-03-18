
def server = Artifactory.server 'Artifactory1'
def buildInfo = Artifactory.newBuildInfo()
buildInfo.env.capture = true
def rtMaven = Artifactory.newMavenBuild()


pipeline {
  
  environment {
    registry = "hassandocker1/my_java_image"
    registryCredential = 'dockerhub'
    kubeconfig = credentials('azure_kubeconfig')
    dockerImage = ''
    
    /**
    def uploadSpec = """{
           "files": [
               {
               "pattern": "target/*.jar",
               "target": "libs-release/"
               }
           ]
    }"""
    */
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
    /**stage('Upload Artifact') {
      steps {
        withCredentials([
                        //usernamePassword(credentialsId: 'Artifactory', usernameVariable: 'ARTIFACTORY_USERNAME', passwordVariable: 'ARTIFACTORY_PASSWORD')
            usernameColonPassword(credentialsId: 'Artifactory', variable: 'credentials')
                        ])
        sh 'curl -u${credentials} -X PUT "http://nisumdevops3c.mylabserver.com:8081/artifactory/libs-release-local/first-spring.jar" -T target/first-spring.jar'
        
        script {
          sh 'curl -u $ARTIFACTORY_USERNAME:$ARTIFACTORY_PASSWORD -X PUT "http://nisumdevops3c.mylabserver.com:8081/artifactory/libs-release-local/first-spring.jar" -T target/first-spring.jar'
        }
      }  
    } */

    stage('Artifactory Configuration') {
      steps {
        //withCredentials([
         //               usernamePassword(credentialsId: 'Artifactory', usernameVariable: 'ARTIFACTORY_USERNAME', passwordVariable: 'ARTIFACTORY_PASSWORD')
            //usernameColonPassword(credentialsId: 'Artifactory', variable: 'credentials')
         //              ])
        //sh 'pwd'
        script {
          
          sh 'ls -lrt'
          sh 'pwd'
          
          rtUpload (
              serverId: 'Artifactory1',
              spec: '''{
                    "files": [
                      {
                        "pattern": "first_spring_boot/target/*.jar",
                        "target": "libs-release/"
                      }
                   ]
              }''',
          )
          
          rtPublishBuildInfo (
                serverId: 'Artifactory1',
                // The buildName and buildNumber below are optional. If you do not set them, the Jenkins job name is used
                // as the build name. The same goes for the build number.
                // If you choose to set custom build name and build number by adding the following buildName and
                // buildNumber properties, you should make sure that previous build steps (for example rtDownload
                // and rtIpload) have the same buildName and buildNumber set. If they don't, then these steps will not
                // be included in the build-info.
          )

          
          /**
          rtMaven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local', server: server
          rtMaven.resolver releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot', server: server
          // rtMaven.deployer.artifactDeploymentPatterns.addExclude("pom.xml")
          //buildInfo = Artifactory.newBuildInfo()
          buildInfo.retention maxBuilds: 10, maxDays: 7, deleteBuildArtifacts: true
          //buildInfo.env.capture = true
          */

          /**
          def server = Artifactory.newServer url: 'http://http://nisumdevops3c.mylabserver.com:8081/artifactory', credentialsId: 'Artifactory'
          server.bypassProxy = true
          def buildInfo = server.upload spec: uploadSpec
          */
          
        }  
      }  
    }
    /**
    stage('Publish build info') {
      steps {
        script {
          server.publishBuildInfo buildInfo
        }  
      }  
    }
    */
 
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
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
    
    stage('Deploy to Kubernetes Azure') {
        steps {
            script {
                //sh 'kubectl create namespace ks-ns --kubeconfig=$kubeconfig --context=MyK8SCluster'
                sh 'kubectl apply -f first_spring_boot/mydeployment_service.yml --kubeconfig=$kubeconfig --context=MyK8SCluster'
                sh 'kubectl get pods --kubeconfig=$kubeconfig --context=MyK8SCluster -n ks-ns'
                sh 'kubectl rollout status deployment myk8sdeployment --kubeconfig=$kubeconfig --context=MyK8SCluster -n ks-ns'
                sh 'kubectl get svc --kubeconfig=$kubeconfig --context=MyK8SCluster -n ks-ns'
            }
        }
    }
  }
}  
  
