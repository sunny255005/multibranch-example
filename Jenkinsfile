pipeline {
    agent any
    tools {
        nodejs 'NODEJS'
    }
    environment {
        user_env_input = 'Development'
        is_invalidate_cache_cloudfront='No'
    }

    stages {

        stage('Which environment to build?') {
            steps {
                script {
                    def userInput = input(id: 'userInput', message: 'Deploy to?',
                    parameters: [[$class: 'ChoiceParameterDefinition', defaultValue: 'Development',
                        description:'Environment choices', name:'denv', choices: 'Development\nTesting']
                    ])
                    
                    
                     def is_invalidate_cache_cloudfront_parameter = input(id: 'is_invalidate_cache_cloudfront', message: 'Do you want to invalidate cache in cloudfront?',
                    parameters: [[$class: 'ChoiceParameterDefinition', defaultValue: 'No',
                        description:'Environment choices', name:'invalidate_cf_params', choices: 'Yes\nNo']
                    ])
                    user_env_input = userInput
                    is_invalidate_cache_cloudfront=s_invalidate_cache_cloudfront_parameter
                    
                //Use this value to branch to different logic if needed
                }
            }
        }
        stage('Confirm') {
            steps {
                input("Do you want to proceed building in ${user_env_input} environment?")
            }
        }

      
        stage('Build UI application')
        {
            steps {

                script {
                    

                     if (user_env_input == 'Testing') {
                        sh 'echo building in Testing  environment'
                        sh 'node --version'
                       
                    }
                    else {
                        sh 'echo building in Development environment'
                        sh 'node --version'
                       
                    }

                }

            }
        }
        stage('Upload to s3 buckets')
        {
            steps {

                script {
                    

                     if (user_env_input == 'Testing') {
                        sh 'echo upload to s3 bucket in  Testing env'
                        
                    }
                    else {
                        sh 'echo upload to s3 bucket in  Development env'
                        
                    }

                }
            }
        }

    stage('Confirm for Invalidate Cache For CLOUDFRONT in ${user_env_input} environment') {
            steps {
               script{
               if(is_invalidate_cache_cloudfront=='Yes')
               {
               stages{
                       stage('Invalidate cache in cloudfront')
    {
            steps {
                script {
                    
                   if (user_env_input == 'Testing') {
                        sh 'echo invalidate cache in Testing env'
                       
                        
                    }
                    else {
                        sh 'echo invalidate cache in Development env'
                         
                       
                    }

                }
            }
    }

     stage("clean ws")
        {
            steps{
            cleanWs deleteDirs: true, notFailBuild: true
        }
        }

            	}
            	else{
            		stage("clean ws")
        {
            steps{
            cleanWs deleteDirs: true, notFailBuild: true
        }
        }
        	}

        }
        }
        }


    }
}
