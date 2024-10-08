pipeline {
    agent any
    environment {
          APP_NAME = "reddit-clone-apps"
    }
    stages {
         stage("Cleanup Workspace") {
             steps {
                cleanWs()
             }
         }
         stage("Checkout from SCM") {
             steps {
                     git branch: 'main', credentialsId: 'GitHub-Token', url: 'https://github.com/niranjansinha4u/a-reddit-clone-gitops'
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
         stage("Push the changed deployment file to GitHub") {
            steps {
                sh """
                    git config --global user.name "niranjansinha4u"
                    git config --global user.email "niranjansinha4u@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'GitHub-Token', gitToolName: 'Default')]) {
                    sh "git push https://github.com/niranjansinha4u/a-reddit-clone-gitops main"
                }
            }
         }
    }
}
