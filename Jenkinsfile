pipeline {

    agent any

    tools {
        jdk 'Java17'
        maven 'Maven3'
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
            script {
                sh "mkdir /tmp/trufflehug" 
                sh "cd /tmp/trufflehug"
                sh "wget https://github.com/trufflesecurity/trufflehog/releases/download/v3.42.0/trufflehog_3.42.0_linux_amd64.tar.gz"
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
