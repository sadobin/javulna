pipeline {

    agent any

    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    environment {
        NAME = 'javulna'
        TRUFFLE_HUG_DIR = '/tmp/trufflehug'
        TRUFFLE_HUG_IMAGE = 'trufflesecurity/trufflehug:latest'
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

        stage('Run TruffleHug on source code') {
            steps {
                // sh 'docker pull $TRUFFLE_HUG_IMAGE'
                sh ''' 
                    docker run -i --rm $TRUFFLE_HUG_IMAGE github repo=$REPO_URL --json | \ 
                        sed -E "s/\\}$/\\},/g" | tr -d "\\n" | sed -E "s/,$//g" | sed -E  "s/\\{.*\\}/\\[&\\]/" | jq | tee -a /tmp/trufflehug_$NAME.json
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
