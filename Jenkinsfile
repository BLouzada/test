
pipeline {
    agent any
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
        stage ('Save test results') {
            steps {
              step([$class: 'JUnitResultArchiver', testResults: 'build/reports/tests/index.html'])
            }
        }
        stage('Deploy to testes') { 
            steps { 
                echo 'deploy to test'
            }
        }
        stage('Sanity check') {
            when {
                expression {
                    BRANCH_NAME == 'master'
                }
            }
             steps {
                input "Does the staging environment for ${env.APP_NAME} look ok?"
             }
         }

         stage('Deploy - Production') {
             when {
                expression {
                    BRANCH_NAME == 'master'
                }
            }
             steps {
               step([$class: 'ArtifactArchiver', artifacts: 'build/libs/demo-0.0.1-SNAPSHOT.jar', fingerprint: true])
             }
         }
    }
}
