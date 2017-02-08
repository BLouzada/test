
pipeline {
    agent any 
    stages {
        stage('Gradle build') { 
            steps { 
                sh './gradlew clean build -X test'
            }
        }
        stage('Testes') { 
            steps { 
                sh './gradlew test'
            }
        }
    }
}
