pipeline {
    agent any

    tools {
        jdk 'jdk21'        // Name from Jenkins Global Tool Configuration
        maven 'maven3'     // Name from Jenkins Global Tool Configuration
    }

    environment {
        // Optional: set SSH credentials ID if using Jenkins SSH plugin
        DEPLOY_SERVER = 'user@192.168.1.100'
        DEPLOY_PATH = '/opt/apps/'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/your-org/your-repo.git', branch: 'main'
            }
        }

        stage('Build Project') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Package JAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Copy JAR to App Server') {
            steps {
                // Option 1: using SCP
                sh 'scp target/*.jar $DEPLOY_SERVER:$DEPLOY_PATH'

                // Option 2 (better): use Publish Over SSH Plugin if configured
                // sshPublisher(publishers: [sshPublisherDesc(configName: 'app-server', transfers: [sshTransfer(sourceFiles: 'target/*.jar', remoteDirectory: '/opt/apps/')], usePromotionTimestamp: false, verbose: true)])
            }
        }
    }

    post {
        success {
            echo '✅ Build and deployment successful!'
        }
        failure {
            echo '❌ Something failed during build or deployment.'
        }
    }
}
