pipeline {
    agent any
    
    stages {
        stage ('Build Image') {
            steps {
                 script {
                      sh "sudo docker build -t brief14 ."       
                 }
                }                
            }
    
        stage ('Push Image') {
            steps {
                script {
                    sh "sudo docker login -u bainos69 -p Supersayien69"        
                    sh "sudo docker push bainos69/brief14"
                }
            }
        }

        stage ('Terraform init Staging') {
            steps {
                script {
                    sh "cd StagingEnvironment && terraform init -upgrade"           
                }    
            }
        } 
        stage ('Terraform apply Staging') {
          steps {
               script {
                    sh "cd StagingEnvironment && terraform ${params.Action} -auto-approve"        
               }
           }
      }
        stage('Sanity check') {
             steps {
                 input "Does the staging environment is ok?"              
            }
        }
        stage ('Terraform init Prod') {
            steps {
                script {
                    sh "cd ProdEnvironment && terraform init -upgrade"                    
                }    
            }
        } 
        stage ('Terraform apply Prod') {
          steps {
               script {
                    sh "cd ProdEnvironment && terraform ${params.Action} -auto-approve"
               }
           }
      }
    }
    
}
