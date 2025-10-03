pipeline {
    agent {
        label "irshad"
    }
    stages {
        stage('code') {
            steps {
                echo "Cloning code..."
                git branch: 'main', url: 'https://github.com/irshad3241/node-calc.git'
            }
        }
        stage('build') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t calc-app:latest ."
            }
        }
        stage('Remove prev-container') {
            steps {
                echo "Removing previous container if exists..."
                sh '''
                docker stop calc-app || true
                docker rm calc-app || true
                '''
            }
        }
        stage('deploy') {
            steps {
                echo "Running container..."
                sh "docker run -d --name calc-app -p 3000:3000 calc-app:latest"
            }
        }
    }
}
