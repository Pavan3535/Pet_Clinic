pipeline {

	agent any

	stages {

		stage('Build') {
			agent {
				dockerfile {
					filename 'Dockerfile.build'
					dir '.'
					args '-d -v $HOME/.m2:/root/.m2 -v $HOME/app:/root/app'
				}
			}
			steps {
				echo "App Built"
			}
		}

		stage('MySQL setup') {
			steps {
				sh 'docker run -d --name petclinic-mysql -e MYSQL_USER=petclinic -e MYSQL_PASSWORD=petclinic -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=petclinic -p 3306:3306 mysql:5.7.8'
			}
		}

		stage('Run') {
			agent {
				dockerfile {
					filename 'Dockerfile.run'
					args '-d -v $HOME/app:/root/app --link petclinic-mysql'
				}
			}
			steps {
				echo 'App Running'
			}
		}

	}
}
