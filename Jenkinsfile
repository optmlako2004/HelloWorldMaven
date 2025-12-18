pipeline {
  agent any

  environment {
    BRANCH = "master"
    TAG_NAME = "v3.${BUILD_NUMBER}"
    GIT_CREDENTIALS = "git-credentials"
  }

  stages {
    stage("Checkout") {
      steps {
        checkout([
          $class: 'GitSCM',
          branches: [[name: "*/${BRANCH}"]],
          userRemoteConfigs: [[
            url: "https://github.com/optmlako2004/HelloWorldMaven.git",
            credentialsId: GIT_CREDENTIALS
          ]]
        ])
      }
    }

    stage("Create Tag") {
      steps {
        withCredentials([usernamePassword(
          credentialsId: GIT_CREDENTIALS,
          usernameVariable: 'GIT_USER',
          passwordVariable: 'GIT_PASS'
        )]) {
          sh """
            git config user.email "christianangelaelako@gmail.com"
            git config user.name "optimalako2004"

            git checkout ${BRANCH}
            git tag -a ${TAG_NAME} -m "Tag créé par Jenkins build ${BUILD_NUMBER}"

            git push https://${GIT_USER}:${GIT_PASS}@github.com/optmlako2004/HelloWorldMaven.git ${TAG_NAME}
          """
        }
      }
    }
  }

  post {
    success {
      echo "Tag ${TAG_NAME} créé avec succès"
    }
    failure {
      echo "Échec création du tag"
    }
  }
}
