pipeline {
    environment {
        AWS_ACCESS_KEY_ID = credentials("AWS_ACCESS_KEY_ID")
        AWS_SECRET_ACCESS_KEY = credentials("AWS_SECRET_ACCESS_KEY")
    }
    agent any
    stages {
        stage('Checkout') {
            steps {
               // Ensure the 'terraform' directory exists and clone the repo
               script {
                  if(!fileExists('terraform')) {
                      sh 'mkdir terraform' 
                    }   
                  }
              dir("terraform") {
                 git branch: 'main', url: 'https://github.com/Anusuya-murugiah/automate-terraform-pjt1.git'                
            }
         }
        }
        stage('Terraform Plan') {
            steps {
                timeout(time: 15, unit: 'MINUTES') {
                   dir('terraform') {
                      sh '''
                      pwd
                      terraform init -input=false -no-color
                      terraform plan -out=tfplan
                      terraform show -no-color tfplan > tfplan.txt
                      '''
                   }
                }
            }
        }
        stage('Debug Workspace') {
    steps {
        sh 'pwd && ls -la'
          }
       }
    }
}
