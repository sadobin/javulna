pipeline {

    agent any

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
