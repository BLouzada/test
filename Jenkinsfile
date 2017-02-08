
node {
    agent any
    def server = Artifactory.newServer url: 'https://testpipeline.jfrog.io/testpipeline/generic-local/', username: 'admin', password: 'B8JSot6yTh'  

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
                def uploadSpec = """{
                  "files": [
                    {
                      "pattern": "build/libs/demo-0.0.1-SNAPSHOT.jar",
                      "target": ""
                    }
                 ]
                }"""
                server.upload(uploadSpec)
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
                 sh 'echo deploying $APP_NAME to production'
             }             
         }
    }
}
