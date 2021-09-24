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

          dockerImage = docker.build registry + ":latest"
        }
        script {
          docker.withRegistry('', registryCredential) {
            dockerImage.push()
          }
        }
        sh "docker rmi $registry:latest"

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
      jiraSendDeploymentInfo environmentId: 'us-prod1', environmentName: 'us-staging1', environmentType: 'testing', issueKeys: [JiraID], serviceIds: [''], site: 'amar-lab.atlassian.net', state: 'successful'
    }
    failure {
      jiraSendDeploymentInfo environmentId: 'us-prod1', environmentName: 'us-staging1', environmentType: 'testing', issueKeys: [JiraID], serviceIds: [''], site: 'amar-lab.atlassian.net', state: 'failed'

    }
  }

}