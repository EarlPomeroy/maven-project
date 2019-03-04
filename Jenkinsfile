pipeline {
	agent any
	triggers {
		pollSCM('* * * * *')
	}
	stages{
		stage('Build'){
			steps {
				sh 'mvn clean package'
				sh "docker -t tomcatwebapp:${env.BUILD_ID} build ."
			}
			post {
				success {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}
	}
}
