pipeline{
    agent {
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "blog-post-microservice-frontend"
        BLOG_NAME = "blog-post-microservice-blogs"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: "main", credentialsId: "github", url: "https://github.com/Nadir24950/blog-post-gitops"
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yml
                    sed -i '21s/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/' deployment.yml
                    cat deployment.yml
                    echo "Done"
                    cat deployment-blogs.yml
                    sed -i '21s/${BLOG_NAME}.*/${BLOG_NAME}:${BLOG_IMAGE_TAG}/' deployment-blogs.yml
                    cat deployment-blogs.yml
                """
            }
        }
        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                    git config --global user.name 'Nadir24950'
                    git config --global user.email 'nadir24950@gmail.com'
                    git add deployment.yml
                    git commit -m 'Updated the Deployment Manifests'
                """

                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/Nadir24950/blog-post-gitops main"
                }
            }
        }
    }
}