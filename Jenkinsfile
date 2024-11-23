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
        git branch: 'main', url: 'https://github.com/Anusuya-murugiah/automate-terraform-pjt1.git'
        }  
      }
    }

    stage('plan') {
      steps {
         sh 'pwd; cd terraform/ ; terraform init'
         sh 'pwd; cd terraform/ ; terraform plan -out tfplan'
         sh 'pwd; cd terraform/ ; terraform show -no-color tfplan > tfplan.txt'
      }  
    }  
  }
}

