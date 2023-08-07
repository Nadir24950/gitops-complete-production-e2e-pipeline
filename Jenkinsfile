pipeline{
    agent {
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "complete-production-e2e-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: "main", credentialsId: "github", url: "https://github.com/Nadir24950/gitops-complete-production-e2e-pipeline"
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yml
                    sed -i '17s/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/' deployment.yml
                    cat deployment.yml
                """
            }
        }
        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                    git config --global user.name 'Nadir24950'
                    git config --global user.email 'nadir24950@gmail.com'
                    git add deployment.yml
                    git commit -m 'Updated the Deployment Manifest'
                """

                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/Nadir24950/gitops-complete-production-e2e-pipeline main"
                }
            }
        }
    }
}
