pipeline {
    agent any
      parameters
    {
         choice choices: ['apply', 'destroy'], name: 'Action'                
    }
    
    /*environment {
        MY_CRED = credentials('f2d10700-72b4-4064-b1d8-1a4882c4f29f')        
     }*/
    
    stages {

         stage ('Clone') {
              steps {
                   checkout scm                        
              }
         }
        
        stage ('Build Image') {
            steps {
                 script {
                      docker.build("bainos69/brief14")        
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

        /*stage ('AZ Login') {
               steps {
                    script {
                         sh "az login --service-principal -u $MY_CRED_CLIENT_ID -p $MY_CRED_CLIENT_SECRET -t $MY_CRED_TENANT_ID"       
                    }
               }
          }*/

        stage ('Terraform init Staging') {
            steps {
                script {
                    sh "cd StagingEnvironment && terraform init -upgrade"           
                }    
            }
        } 
        stage ('Terraform apply/destroy Staging') {
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
        stage ('Terraform apply/destroy Prod') {
          steps {
               script {
                    sh "cd ProdEnvironment && terraform ${params.Action} -auto-approve"
               }
           }
      }
    }
    
}
