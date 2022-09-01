pipeline {
    agent any
    tools {
        nodejs 'NODEJS'
    }
    environment {
        user_env_input = 'Development'
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
                        sh 'node --version'
                        sh 'npm install'
                        sh 'ng build'
                    }
                    else {
                        sh 'echo building in Development environment'
                        sh 'node --version'
                        sh 'npm install'
                        sh 'npm run build-dev'
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
       
        stage('Confirm') {
            steps {
                input("Do you want to proceed invalidate cache CLOUDFRONT in ${user_env_input} environment?")
            }
        }
        
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
}
