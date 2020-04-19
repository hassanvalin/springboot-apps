Spring Boot Apps

// Update your git repo with forked git repo -- https://medium.com/@topspinj/how-to-git-rebase-into-a-forked-repo-c9f05e821c8a



- By default Jenkins installation creates an user called 'jenkins' and all jobs would be executed with that user. So, you have to enable sudo access to jenkins user by updating 'visudo'. 

sudo visudo -> update the sudoers with entry `jenkins	ALL=(ALL) 	NOPASSWD: ALL`


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



