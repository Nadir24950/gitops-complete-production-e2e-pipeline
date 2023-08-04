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
                git branch: "master", credentialsId: "github", url: "https://githu.com/Nadir24950/jenkins-java-gitops"
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAGE}/g' deployment.yml
                    cat deployment.yml
                """
            }
        }
        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                    git config --global user.name "Nadir24950"
                    git config --global user.email "nadir24950@gmail.com"
                    git add deployment.yml
                    git commit -m "Updated the Deployment Manifest
                """

                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/Nadir24950/jenkins-java-gitops master"
                }
            }
        }
    }
}