@Library("sharedLib") _
pipeline {
    agent {
        label "irshad"
    }
    stages {
        stage('Code') {
            steps {
                script{
                    clone('https://github.com/irshad3241/node-calc.git','main')
                }
            }
        }
        stage('Build') {
            steps {
                script{
                    build('calc-app','latest')
                }
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
                    sh 'docker image prune -f'
                }
            }
        }
        stage('Deploy') {
            steps {
                echo "Running container..."
                sh "docker run -d --name calc-app -p 3000:3000 irshadshaikh63/calc-app:latest"
            }
        }
    }
}