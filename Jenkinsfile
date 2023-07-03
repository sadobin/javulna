pipeline {

    agent any

    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    environment {
        TRUFFLE_HUG_DIR = '/tmp/trufflehug'
    }
    
    stages {
        
        stage('cleanup workspace') {
            steps {
                cleanWs()
            }
        }

        stage('checkout from SCM') {
            steps {
                git branch: 'master',
                    credentialsId: 'javulna-pat',
                    url: 'https://github.com/sadobin/javulna.git'
            }
        }

        stage('run TruffleHug on source code') {
            steps {
                script {
                    sh """
                    mkdir -p ${TRUFFLE_HUG_DIR}/src 
                    #apt update
                    apt install wget
                    cd ${TRUFFLE_HUG_DIR}/src && \
                        wget https://github.com/trufflesecurity/trufflehog/releases/download/v3.42.0/trufflehog_3.42.0_linux_amd64.tar.gz && \
                        tar xfz trufflehog_3.42.0_linux_amd64.tar.gz
                    mv ${TRUFFLE_HUG_DIR}/src/trufflehug ${TRUFFLE_HUG_DIR}
                    """
                }
            }
        }
        
        stage('package the app') {
            steps {
                sh "mvn clean package"
            }
        }

        stage('test the app') {
            steps {
                sh "mvn test"
            }
        }
        
    }

}
