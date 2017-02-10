
pipeline {
    agent none
    stages {
        stage('Gradle build') { 
            agent { label 'master' }
            steps { 
                sh './gradlew clean build -x test'
            }
        }
        stage('Run tests') { 
        agent { label 'master' }
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
        agent { label 'master' }
            steps { 
                echo 'deploy to test'
            }
        }
        stage('Sanity check') {
        agent none
            when {
                expression {
                    BRANCH_NAME == 'master'
                }
            }
             steps {
                input message: 'Does the staging environment for ${env.JOB_NAME} look ok?', submitter: 'admin', submitterParameter: 'test'
             }
         }

         stage('Deploy - Production') {
         agent { label 'master' }
             when {
                expression {
                    BRANCH_NAME == 'master'
                }
            }
             steps {
             script {
                def server = Artifactory.server 'jfrog'
                def uploadSpec = """{
                  "files": [
                    {
                      "pattern": "build/libs/demo-0.0.1-SNAPSHOT.jar",
                      "target": "build"
                    }
                 ]
                }"""
                server.upload(uploadSpec)
                }
               step([$class: 'ArtifactArchiver', artifacts: 'build/libs/demo-0.0.1-SNAPSHOT.jar', fingerprint: true])
             }
         }
    }
    /*post {
        always {
              step([$class: 'JUnitResultArchiver', testResults: 'build/test-results/TEST-*.xml'])
            }
        }*/
}
