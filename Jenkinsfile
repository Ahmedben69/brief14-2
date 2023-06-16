pipeline {
    agent any
    
    stages {
        stage ('Build Image') {
            steps {
                 script {
                      sh "docker build -t brief14 ."       
                 }
                }                
            }
    
        stage ('Push Image') {
            steps {
                script {
                    sh "docker login -u bainos69 -p dckr_pat_TjpHATmjKOBWT57D5CPv7AoPQtw"        
                    sh "docker push bainos69/brief14"
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
