pipeline {

    agent any

    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    environment {
        NAME = 'javulna'
        TRUFFLE_HOG_DIR = '/tmp/trufflehog'
        TRUFFLE_HOG_IMAGE = 'trufflesecurity/trufflehog:latest'
        REPO_URL = 'https://github.com/sadobin/javulna.git'
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
                    url: "$REPO_URL"
            }
        }

        stage('Run TruffleHog on source code') {
            steps {
                // sh 'docker pull $TRUFFLE_HOG_IMAGE'
                sh ''' 
                    result=$(docker run -i --rm $TRUFFLE_HOG_IMAGE git $REPO_URL -j)
                    echo $result | tr -d '\\r' | sed -E "s/\\}$/\\},/g" | tr -d "\\n" | sed -E "s/,$//g" | sed -E  "s/\\{.*\\}/\\[&\\]/" | jq | tee -a /tmp/trufflehog_$NAME.json
                '''
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
