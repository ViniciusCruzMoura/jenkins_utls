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
                sh 'docker compose build'
            }
        }

		stage('Tag') {
			steps {
				withCredentials([usernamePassword(credentialsId: '123-example-456', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
					sh('git tag -a v$BUILD_NUMBER -m "Tag created from Jenkins for build $BUILD_NUMBER" ')
					sh('git push https://$GIT_USERNAME:$GIT_PASSWORD@$(echo $GIT_URL | sed "s|https://||;") v$BUILD_NUMBER')
				}
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
