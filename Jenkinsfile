pipeline {
  environment {
    AWS_ACCESS_KEY_ID = credentials("AWS_ACCESS_KEY_ID")
    AWS_SECRET_ACCESS_KEY = credentials("AWS_SECRET_ACCESS_KEY")
  }
  agent any
  stages {
    stage('checkout') {
      steps {
        dir("terraform") {
        git "https://github.com/Anusuya-murugiah/automate-terraform-pjt1.git"
        }  
      }
    }
  }
  
}

