pipeline {
    agent { label 'runner2' }

    parameters {
        gitParameter name: 'BRANCH_TAG',
                     type: 'PT_BRANCH_TAG',
                     defaultValue: 'dev',
                     sortMode: 'ASCENDING_SMART',  
                     listSize: '10'    
    }

    environment {
    //    DEPLOY_SERVER = "dockerizer.navek.dev"
        DEPLOY_SERVER_IP = "192.168.200.103"
        DOCKER_HUB_CREDS = credentials('dockerhub_credentials')
        PORT = "${BUILD_NUMBER.toInteger() + 8000}"
    }

    stages {
       stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: "${params.BRANCH_TAG}"]],
                          doGenerateSubmoduleConfigurations: false,
                          extensions: [],
                          gitTool: 'Default',
                          submoduleCfg: [],
                          userRemoteConfigs: [[url: 'https://github.com/rkozello/git-parameter.git']]
                        ])
            }
        }
        stage('Show Env') {
            steps {
                sh '''
                printenv
                cat README.md
                '''
            }
        }
    }

    post {
        always {
            cleanWs()            
            dir("${env.WORKSPACE}@tmp") {
                deleteDir()
            }
        }        
    }
}