pipeline {
    agent any

    tools {
        maven 'maven'
    }

    environment {
        MAVEN_OPTS = "-Xmx512m"
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    echo "Checking out branch or tag: ${params.BRANCH}"
                    checkout([$class: 'GitSCM', branches: [[name: "${params.BRANCH}"]], userRemoteConfigs: [[url: REPO_URL]]])
                }
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/hello-world-1.0-SNAPSHOT.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "Build was successful!"
        }
        failure {
            echo "Build failed!"
        }
    }
}
