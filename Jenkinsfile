pipeline {
    agent {
        label "irshad"
    }
    stages {
        stage('Code') {
            steps {
                echo "Cloning code..."
                git branch: 'main', url: 'https://github.com/irshad3241/node-calc.git'
            }
        }
        stage('Build') {
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
        stage('Push') {
            steps {
                echo "Pushing to Docker Hub..."
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerPass', usernameVariable: 'dockerUser')]) {
                    sh 'docker login -u $dockerUser -p $dockerPass'
                    sh 'docker image tag calc-app:latest irshadshaikh63/calc-app:latest'
                    sh 'docker push $dockerUser/calc-app:latest'
                }
            }
        }
        stage('Deploy') {
            steps {
                echo "Running container..."
                sh "docker run -d --name calc-app -p 3000:3000 calc-app:latest"
            }
        }
    }
}
