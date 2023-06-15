pipeline {
    agent any
    
    environment {
        MY_CRED = credentials('f2d10700-72b4-4064-b1d8-1a4882c4f29f')
     }
    
    stages {

         stage ('Clone') {
              steps {
                   checkout scm
              }
         }
        
        stage ('Build Image') {
            steps {
                 script {
                      docker.build("mowqa/pytoon")
                 }
                }                
            }
    
        stage ('Push Image') {
            steps {
                script {
                    sh "docker login -u mowqa -p dckr_pat_is0y3bHt8AoE6BLlA7sv3NaKJMI"
                    sh "docker push mowqa/pytoon"
                }
            }
        }

        stage ('AZ Login') {
               steps {
                    script {
                         sh "az login --service-principal -u $MY_CRED_CLIENT_ID -p $MY_CRED_CLIENT_SECRET -t $MY_CRED_TENANT_ID"
                    }
               }
          }

        stage ('Terraform init') {
            steps {
                script {
                    sh "cd StagingEnvironment && terraform init -auto-approve"
                }    
            }
        } 
        stage ('Terraform apply') {
          steps {
               script {
                    sh "cd StagingEnvironment && terraform apply"
               }
           }
      }
        stage('Sanity check') {
             steps {
                 input "Does the staging environment look ok?"
            }
        }
        stage ('Terraform init') {
            steps {
                script {
                    sh "cd ProdEnvironment && terraform init -auto-approve"
                }    
            }
        } 
        stage ('Terraform apply') {
          steps {
               script {
                    sh "cd ProdEnvironment && terraform apply"
               }
           }
      }
    }
    
}
