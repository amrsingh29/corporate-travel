pipeline {
  environment {
    registry = "amrsingh1/corporate-travel"
    registryCredential = 'dockerhub_id'
    dockerImage = ""
  }

  agent any
  stages {
    stage('BUILD STAGE') {
      steps {

        git branch: 'main', url: 'https://github.com/amrsingh29/corporate-travel.git'
        script {

          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
        script {
          docker.withRegistry('', registryCredential) {
            dockerImage.push()
          }
        }
        sh "docker rmi $registry:$BUILD_NUMBER"

      }

    }

    stage("TEST STAGE") {
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