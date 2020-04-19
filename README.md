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


- 'parameters' directive provides a list of parametes that a user should provide when triggering the pipeline. The values for these user-specified parameters are made available to Pipeline steps via the 'params' object.

parameters {
        string(name: 'PERSON', defaultValue: 'DAN', description: 'Give name of the person')
        text(name: 'ANYTEXT', defaultValue: '', description: 'Enter any text')
        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Enter True or False')
        choice(name: 'MYCHOICE', choices: ['Hello','Hi','How r u'], description: 'Select your choice')
        password(name: 'MYPWD', defaultValue: 'SECRET', description: 'Enter the password')
}
    
stage('Taking parameters') {
            steps {
                print("Hello, ${params.PERSON}")
                print("${params.ANYTEXT}")
                print("${TOGGLE}")
                print("My choice is: ${MYCHOICE}")
                print("My password is: ${MYPWD}")
            }
 }    

