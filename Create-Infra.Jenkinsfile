pipeline {
    agent any 
    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Select The Environment')
    }
    options {
        ansiColor('xterm')    // Add's color to the output : Ensure you install AnsiColor Plugin.
    }
        stages {
                stage('Terraform Create Network') {
                    steps {
                        git branch: 'main', url: 'https://github.com/Adarsh-Pixel/terraform-vpc.git'
                                sh "terrafile -f env-${ENV}/Terrafile"
                                sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars  -reconfigure"
                                sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                                sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
                        }
                    }
                stage('Terraform Create ALB') {
                    steps {
                        git branch: 'main', url: 'https://github.com/Adarsh-Pixel/terraform-loadbalancers.git'
                                sh "terrafile -f env-${ENV}/Terrafile"
                                sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure"
                                sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                                sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
                        }
                    }

                stage('Terraform Create Databases') {
                    steps {
                                git branch: 'main', url: 'https://github.com/Adarsh-Pixel/terraform-databases.git'
                                sh "terrafile -f env-${ENV}/Terrafile"
                                sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure"
                                sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                                sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
                            }
                        }   
            stage('Backend') {
            parallel {
                stage('Terraform Create Catalogue') {
                    steps {
                        dir('catalogue')   { git branch: 'main', url: 'https://github.com/Adarsh-Pixel/catalogue.git'
                                sh '''
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100 -auto-approve
                                '''
                        }
                    }
                } 
                stage('Terraform Create user') {
                    steps {
                        dir('user')   { git branch: 'main', url: 'https://github.com/Adarsh-Pixel/user.git'
                                sh '''
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100 -auto-approve
                                '''
                        }
                    } 
                }
                stage('Terraform Create cart') {
                    steps {
                        dir('cart')   { git branch: 'main', url: 'https://github.com/Adarsh-Pixel/cart.git'
                                sh '''
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100 -auto-approve
                                '''
                        }
                    } 
                }
                stage('Terraform Create shipping') {
                    steps {
                        dir('shipping')   { git branch: 'main', url: 'https://github.com/Adarsh-Pixel/shipping.git'
                                sh '''
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100 -auto-approve
                                '''
                            }
                        } 
                }
                stage('Terraform Create payment') {
                    steps {
                            dir('payment')   { git branch: 'main', url: 'https://github.com/Adarsh-Pixel/payment.git'
                                    sh '''
                                        cd mutable-infra
                                        terrafile -f env-${ENV}/Terrafile
                                        terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                        terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100
                                        terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100 -auto-approve
                                    '''
                                }
                            } 
                        }
                    }
                } 
        // stage('Terraform Create frontend') {
        //     steps {
        //             dir('frontend')   { git branch: 'main', url: 'https://github.com/Adarsh-Pixel/frontend.git'
        //                     sh '''
        //                         cd mutable-infra
        //                         terrafile -f env-${ENV}/Terrafile
        //                         terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
        //                         terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100
        //                         terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=100 -auto-approve
        //                     '''
        //             }
        //     }
        // }    
    } 
}