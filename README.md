Spring Boot Apps

// Update your git repo with forked git repo -- https://medium.com/@topspinj/how-to-git-rebase-into-a-forked-repo-c9f05e821c8a



- By default Jenkins installation creates an user called 'jenkins' and all jobs would be executed with that user. So, you have to enable sudo access to jenkins user by updating 'visudo'. 

sudo visudo -> update the sudoers with entry `jenkins	ALL=(ALL) 	NOPASSWD: ALL`


---
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

---
- parameters

'parameters' directive provides a list of parametes that a user should provide when triggering the pipeline. The values for these user-specified parameters are made available to Pipeline steps via the 'params' object.

pipeline {
    agent any
    
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }
    stages {
        stage('Example') {
            steps {
                echo "Hello ${params.PERSON}"

                echo "Biography: ${params.BIOGRAPHY}"

                echo "Toggle: ${params.TOGGLE}"

                echo "Choice: ${params.CHOICE}"

                echo "Password: ${params.PASSWORD}"
            }
        }
    }
}


---
- tools

'tools' section is to auto-install and put on the PATH. 

Supported tools : maven, gradle, jdk

pipeline {
    agent any
    
    tools {
        maven 'apache-maven-3.0.1'  // The name 'apache-maven-3.0.1' should be configured in Jenkins global tools configuration before using here
    }
    stages {
        stage('Example') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}


---
- input

The 'input' directive on a stage allows you to prompt for input, using the input step.

Configurations Options:

message ,
ok , 
submitter , 
parameters -- Make sure while calling in steps we are not using params object , we can directly use varaiable name with ${}

pipeline {
    agent any
    
    stages {
        stage('Example') {
            input {
                message "Should we continue?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps {
                echo "Hello, ${PERSON}, nice to meet you."
            }
        }
    }
}


---
- when

The 'when' directive allows the Pipeline to determine whether the stage should be executed depending on the given condition. The when directive must contain at least one condition. If the when directive contains more than one condition, all the child conditions must return true for the stage to execute. This is the same as if the child conditions were nested in an 'allOf' condition (see the examples below). If an 'anyOf' condition is used, note that the condition skips remaining tests as soon as the first "true" condition is found.

We have many buit in conditions to check with when directive ... 
