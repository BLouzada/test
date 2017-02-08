
pipeline {
    agent any 
    GIT_BRANCH = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
    stages {
        stage('Gradle build') { 
            steps { 
                sh './gradlew clean build -x test'
            }
        }
        stage('Run tests') { 
            steps { 
                parallel (
                    "Test" : {
                        sh './gradlew test'
                    },
                    "Echo" : {
                        echo 'ola mundo'
                    }
                )
            }
        }
        stage('Deploy to testes') { 
            steps { 
                sh 'cp build/libs/demo-0.0.1-SNAPSHOT.jar .'
            }
        }
        stage('Sanity check') {
        when {
                expression { return GIT_BRANCH.equals("master") }
            }
             steps {
                 input "Does the staging environment for ${env.APP_NAME} look ok?"
             }
         }

         stage('Deploy - Production') {
             when {
               expression { return GIT_BRANCH.equals("master") }
            }
             steps {
                 sh 'echo deploying $APP_NAME to production'
             }             
         }
    }
}
