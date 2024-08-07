pipeline {
    agent any 
    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Select The Environment')
    }
    options {
        ansiColor('xterm')    // Add's color to the output : Ensure you install AnsiColor Plugin.
    }
    stages {

            stage('Destroying-Shipping') {
                steps {
                    dir('SHIPPING') {  git branch: 'main', url: 'https://github.com/Adarsh-Pixel/shipping.git'
                          sh '''
                            cd mutable-infra
                            terrafile -f env-${ENV}/Terrafile
                            terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform destroy -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=100  -auto-approve
                          '''
                            }
                        }
                    }

            stage('Destroying-User') {
                   steps {
                       dir('USER') {  git branch: 'main', url: 'https://github.com/Adarsh-Pixel/user.git'
                          sh '''
                            cd mutable-infra
                            terrafile -f env-${ENV}/Terrafile
                            terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform destroy -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=100 -auto-approve
                          '''
                            }
                        }
                   }
                   
            stage('Destroying-Catalogue') {
                   steps {
                       dir('Catalogue') {  git branch: 'main', url: 'https://github.com/Adarsh-Pixel/catalogue.git'
                          sh '''
                            cd mutable-infra
                            terrafile -f env-${ENV}/Terrafile
                            terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform destroy -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=100 -auto-approve
                          '''
                            }
                        }
                  }
            stage('Destroying-Payment') {
                steps {
                    dir('PAYMENT') {  git branch: 'main', url: 'https://github.com/Adarsh-Pixel/payment.git'
                          sh '''
                            cd mutable-infra
                            terrafile -f env-${ENV}/Terrafile
                            terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform destroy -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=100 -auto-approve
                          '''
                         }
                     }
                }

            stage('Destroying-Cart') {
                steps {
                    dir('CART') {  git branch: 'main', url: 'https://github.com/Adarsh-Pixel/cart.git'
                          sh '''
                            cd mutable-infra
                            terrafile -f env-${ENV}/Terrafile
                            terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform destroy -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=100 -auto-approve
                          '''
                         }
                     }
                }
                

            stage('Destroying-Frontend') {
                steps {
                    dir('frontend') {  git branch: 'main', url: 'https://github.com/Adarsh-Pixel/frontend.git'
                          sh '''
                            cd mutable-infra
                            terrafile -f env-${ENV}/Terrafile
                            terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform destroy -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=100 -auto-approve
                          '''
                         }
                     }
                }

        stage('Terraform Destroy Databases') {
            steps {
                        git branch: 'main', url: 'https://github.com/Adarsh-Pixel/terraform-databases.git'
                        sh "terrafile -f env-${ENV}/Terrafile"
                        sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure"
                        sh "terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
                    }
                }

        stage('Terraform Destroy ALB') {
            steps {
                git branch: 'main', url: 'https://github.com/Adarsh-Pixel/terraform-loadbalancers.git'
                        sh "terrafile -f env-${ENV}/Terrafile"
                        sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure"
                        sh "terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
                }
            }


        stage('Terraform Destroy Network') {
            steps {
                git branch: 'main', url: 'https://github.com/Adarsh-Pixel/terraform-vpc.git'
                        sh "terrafile -f env-${ENV}/Terrafile"
                        sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars  -reconfigure"
                        sh "terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
                    }
                }                    
            }    
        }                        