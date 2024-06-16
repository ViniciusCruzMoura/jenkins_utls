pipeline {
    agent {
        docker {
            image 'docker:cli'
        }
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'docker ps --all'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                sh 'pip install requests'
                sh 'python main.py'
                sh 'cat /etc/*-release'
                sh 'ls -la'
                sh 'pwd'
            }
        }
    }
}
