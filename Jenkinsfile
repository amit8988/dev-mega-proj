
pipeline {
    agent any

    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from SCM') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Devops1224789/Devops-Mega-project.git'
            }
        }

        stage('Build the application') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test the application') {
            steps {
                sh 'mvn test'
            }
        }

        // ✅ Corrected SonarQube Analysis Stage
        stage('Sonarqube Analysis') {
            steps {
                script {
                    // Replace 'sonarqube-server' with your actual SonarQube server name configured in Jenkins
                    withSonarQubeEnv('sonarqube-server') {
                        withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                            sh 'mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN'
                        }
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        failure {
            echo "❌ Build failed!"
        }
        success {
            echo "✅ Build succeeded!"
        }
    }
}
