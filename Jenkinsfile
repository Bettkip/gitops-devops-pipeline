pipeline {
    agent {
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "devops-pipeline"
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                clearWs ()
            }
        }

          stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Bettkip/gitops-devops-pipeline'
            }
        }


          stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                  """
            }
        }


          stage("Push the changed deployment file to git") {
            steps {
                sh """
                    git config --global user.name "Bettkip"
                    git config --global user.email "bettkipkorirf@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                  """
                  withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/Bettkip/gitops-devops-pipeline main"
                  }

            }
        }
    }
}
