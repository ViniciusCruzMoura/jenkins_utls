pipeline {
    agent {
        label 'prod && srv085041'
    }

    environment {
        env_file = credentials('cardcredenciamento_env')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        
        stage('Test') {
            agent {
                docker {
                    image 'python:3.9'
                    label 'prod && srv085041'
                }
            }
            steps {
                echo 'Testing..'
                sh 'cat $env_file > .env'
                sh 'pip install --no-cache-dir -r dependencies/base.txt'
                sh 'python -m unittest discover tests/'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
