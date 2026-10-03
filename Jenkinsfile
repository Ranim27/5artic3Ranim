pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                dir('backend') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sq1') {
                    dir('backend') {
                        sh 'mvn sonar:sonar -Dsonar.projectKey=gestion-projets'
                    }
                }
            }
        }
    }
}
