pipeline {
    parameters {
       booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply after generating the plan')
       booleanParam(name: 'destroyResources', defaultValue: false, description: 'destroy resources after creation?')
    }
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
                 dir("terraform") {
                    git branch: 'main', url: 'https://github.com/Anusuya-murugiah/automate-terraform-pjt1.git'                
                  }
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
        stage('Approval') {
           when {
              not {
                  equals expected:true, actual: params.autoApprove
              }
           }
           steps {
              script {
                 def plan = readFile ('terraform/tfplan.txt')
                 input message: "Do you want apply the plan?",
                 parameters: [text(name:'Plan', defaultValue: plan, description: 'please review the plan')]
              }
           }
        }
        stage('Apply') {
           steps {
              dir('terraform') {
                  sh 'terraform apply -input=false tfplan'
              }
           }      
       }
       stage('destroy') {
           when {
               equals expected:true, actual: params.destroyResources
           }    
           steps {
              dir('terraform') {
                 sh 'terraform destroy -auto-approve'
              }
           }    
         }
       }
    }
