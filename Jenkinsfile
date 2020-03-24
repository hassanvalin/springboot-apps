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
            mvn clean install -DskipTests
            '''
        }  
      }
    }
    
    /**
    stage('Artifactory Upload') {
      steps {
        //withCredentials([
             //          usernamePassword(credentialsId: 'Artifactory', usernameVariable: 'ARTIFACTORY_USERNAME', passwordVariable: 'ARTIFACTORY_PASSWORD')
            //usernameColonPassword(credentialsId: 'Artifactory', variable: 'credentials')
            //           ])
        script {
          
          sh 'ls -lrt'
          sh 'pwd'
          
          rtUpload (
              serverId: 'Artifactory1',
              spec: '''{
                    "files": [
                      {
                        "pattern": "first_spring_boot/target/*.jar",
                        "target": "libs-release/$BUILD_NUMBER/"
                      }
                   ]
              }''',
          )
          
          rtPublishBuildInfo (
                serverId: 'Artifactory1',
          )

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
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
   
    /** 
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
    } */

    stage('Deploy Using Helm') {
	steps {
	    script {
                sh 'pwd'
		sh '/usr/local/bin/helm install my-app helm_first_app --kubeconfig=$kubeconfig --kube-context=MyK8SCluster'
                sh '/usr/local/bin/helm list --kubeconfig=$kubeconfig --kube-context=MyK8SCluster'
            }
        }	
    }
  }
}  
  
