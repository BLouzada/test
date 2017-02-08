
pipeline {
    agent any 
    stages {
        stage('Gradle build') { 
            steps { 
                sh './gradlew clean build -x test'
            }
        }
        stage('Testes') { 
            steps { 
                sh './gradlew test'
            }
        }
    }
}
