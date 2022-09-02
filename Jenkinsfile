pipeline {
    agent any
    
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
                    
                    
                    
                    user_env_input = userInput
                    
                    
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
                       
                       
                    }
                    else {
                        sh 'echo building in Development environment'
                      
                       
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



    stage('Confirm for Invalidate Cache For CLOUDFRONT') {
            steps {
                 
               script{
                   def is_invalidate_cache_cloudfront_parameter = input(id: 'is_invalidate_cache_cloudfront', message: 'Do you want to invalidate cache in cloudfront?',
                    // parameters: [[$class: 'ChoiceParameterDefinition', defaultValue: 'No',
                    //     description:'Environment choices', name:'invalidate_cf_params', choices: 'Yes\nNo']
                    // ])
                    parameters: [[$class: 'booleanParam', defaultValue: 'true',
                        description:'Environment choices', name:'invalidate_cf_params']
                    ])
                   // parameters([booleanParam(defaultValue: true, description: 'Cloudfront  choice', name: 'invalidate_cf_params')])])
                    is_invalidate_cache_cloudfront=is_invalidate_cache_cloudfront_parameter


                      when {
                expression {
                    return params.invalidate_cf_params
                }
            }
            steps {
                echo "Hello World!"
            }

               }}}



        
        



    


     stage("clean ws")
        {
            steps{
            cleanWs deleteDirs: true, notFailBuild: true
        }
        }
           
}
    }

