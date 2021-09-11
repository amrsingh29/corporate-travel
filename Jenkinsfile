pipeline {
	environment {
		registry = "amrsingh1/bankingapp"
		registryCredential = 'dockerhub_id'
		dockerImage = ""
	}
	
	agent any
	stages {
		stage('Cloning our Git') {
			steps{
			    git(branch : 'main',
			        credentialsId: 'github_id' ,
			        url: 'https://github.com/amrsingh29/docker-jenks-build.git')
			
			}
		}
		
		stage('Building our image') {
			steps {
			    script {
			        
			        dockerImage = docker.build registry + ":$BUILD_NUMBER"
			    }
				
			}
		}
		
		stage('Deploy our image') {
			steps {
				script {
					docker.withRegistry( '', registryCredential ) {
						dockerImage.push()
					}
				}
			}
		}
		
		stage('Cleaning up') {
			steps {
				sh "docker rmi $registry:$BUILD_NUMBER"
			}
		}
		
		stage("TEST") {
            steps {
                echo "I am running test stage!!!"
                 }
        }

	}
			 post {

            success {
                    echo "Post stage running on Success!!!"
                    echo env.BUILD_NUMBER
                    echo env.JOB_NAME
					jiraSendDeploymentInfo environmentId: 'us-prod1', environmentName: 'us-prod1', environmentType: 'production', issueKeys: [JiraID], serviceIds: [''], site: 'amar-lab.atlassian.net', state: 'successful'
            }
            failure {
                jiraSendDeploymentInfo environmentId: 'us-prod1', environmentName: 'us-prod1', environmentType: 'production', issueKeys: [JiraID], serviceIds: [''], site: 'amar-lab.atlassian.net', state: 'failed'
                
            }
        }

}