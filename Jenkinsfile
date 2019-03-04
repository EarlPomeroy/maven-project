pipeline {
	agent any
	parameters {
		string(name: 'tomcat_staging', defaultValue: '54.202.148.49', description: 'Staging server')
		string(name: 'tomcat_production', defaultValue: '35.166.13.137', description: 'Production server')
	}
	triggers {
		pollSCM('* * * * *')
	}
	stages{
		stage('Build'){
			steps {
				sh 'mvn clean package'
			}
			post {
				success {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}
	  stage ('Deployments'){
			parallel {
		    stage ('Deploy to Staging'){
	        steps {
	            sh "scp -i /var/jenkins_home/svr.pem **/target/*.war ec2-user@${params.tomcat_staging}:/var/lib/tomcat7/webapps"
	        }
		    }
		    stage ("Deploy to Production"){
	        steps {
	            sh "scp -i /var/jenkins_home/svr.pem **/target/*.war ec2-user@${params.tomcat_production}:/var/lib/tomcat7/webapps"
	        }
		    }
			}
	  }
	}
}
