pipeline{
  agent{ label "Jenkins-Agent"}
  environment{
    APP_NAME = "register-app-pipe"
  }
  stages{
    stage(Cleanup WorkSpace){
      steps{
        cleanWs()
      }
    }
    stage("Checkout from SCM"){
      steps{
        git branch: 'main' , credentialsId: 'Github' , url = 'https://github.com/mandals0908/gitops-register-app.git'
      }
    }
    stage("Update the deployment Tags"){
      steps{
        sh """
        cat deployment.yaml
        sed -i '/s${APP_NAME}.*/${APP_NAME}:{IMAGE_TAG}/g' deployment.yaml
        cat deployment.yaml
        """
      }
    }
    stage("Push the changed deployment file to Git"){
      steps{
        sh """
        git config --global user.name "mandals0908"
        git config --global user.email "mandals0908@gmail.com"
        git add deployment.yaml
        git commit -m "Updated deployment manifest"
        """
        WithCredentials([gitUsernamePassword(credentialsId: 'Github' , gitToolName: 'Default')]){
          sh "git push "https://github.com/mandals0908/gitops-register-app main" 
        }
      }
    }
  }
}
