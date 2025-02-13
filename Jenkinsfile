pipeline {
    agent any

    stages {
        stage('Cloning the project repository from BitBucket') {
            steps {
                checkout(
                    [
                        $class: 'GitSCM', 
                        branches: [[name: '*/master']], 
                        doGenerateSubmoduleConfigurations: false, 
                        extensions: [[$class: 'CleanBeforeCheckout']], 
                        submoduleCfg: [], 
                        userRemoteConfigs: [
                            [
                                credentialsId: 'git', 
                                url: 'https://github.com/sajjasgit/packer-immutable-ami-demo.git'
                            ]
                        ]
                    ]
                )
            }
        }

        stage('building golden ami using packer') {
            steps {
                withCredentials([[
                $class: 'AmazonWebServicesCredentialsBinding',
                accessKeyVariable: 'AWS_ACCESS_KEY_ID', // dev credentials
                credentialsId: 'Ashrujit-DevOps',
                secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]){
                    sh '''
                        $rootdir = (Resolve-Path .\\).Path
                        Set-Location $rootdir\\packer
                        packer build -var "aws_access_key=$($ENV:AWS_ACCESS_KEY_ID)" -var "aws_secret_key=$($ENV:AWS_SECRET_ACCESS_KEY)" packer.json
                    '''
                }
            }
        }
    }
}
