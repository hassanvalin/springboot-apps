Spring Boot Apps

// Update your git repo with forked git repo -- https://medium.com/@topspinj/how-to-git-rebase-into-a-forked-repo-c9f05e821c8a



- Simple code to checkout with Jenkins file

pipeline {
    agent any
    
    stages {
        stage('checkout') {
            steps {
                git branch: 'master',  credentialsId: 'Git_Hub', url: 'https://github.com/hassanvalin/Python_Practice.git'
            }
        }
    }
}
