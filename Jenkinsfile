pipeline {
    agent any

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git url: 'https://github.com/titasuddin/laravel-app-cd.git', branch: 'main'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                
                   sed -i "s|image: titasuddin/laravel-app:.*|image: titasuddin/laravel-app:${IMAGE_TAG}|g" deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "titasuddin"
                   git config --global user.email "titasuddin@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/titasuddin/laravel-app-cd main"
                }
            }
        }
      
    }
}
