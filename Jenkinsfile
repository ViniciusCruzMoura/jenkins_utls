pipeline {
    agent {
        label 'prod && srv085041'
    }

    environment {
        env_file = credentials('cardterminais_env')
    }

    stages {
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'cat $env_file > .env'
                sh 'docker build --network=host -t cardterminais:test .'
                sh 'docker run --rm --network=host --env-file .env cardterminais:test sh -c "python -m unittest discover tests/"'
                sh 'docker rmi cardterminais:test --force'
            }
        }
		
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'cat $env_file > .env'
                sh 'docker compose build'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying....'
                sh 'cat $env_file > .env'
                sh 'docker compose down'
                sh 'docker compose up -d'
            }
        }
    }
}
