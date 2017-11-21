pipeline {
    agent any
    parameters {
        string(name: 'DTR_IP', defaultValue: 'dtr.erictankok.dtcntr.net')
        string(name: 'UCP_USER', defaultValue: 'ucpadmin')
        string(name: 'UCP_PASSWORD', defaultValue: 'ucpadmin')
        string(name: 'DOCKER_CONTENT_TRUST', defaultValue: '0')
        string(name: 'DOCKER_CONTENT_TRUST_ROOT_PASSPHRASE', defaultValue: '')
        string(name: 'DOCKER_CONTENT_TRUST_REPOSITORY_PASSPHRASE', defaultValue: '')
        string(name: 'HOST_PORT', defaultValue: '4000')
        string(name: 'CONTAINER_PORT', defaultValue: '4000')
    }
    environment {
        DOCKER_CONTENT_TRUST = "${params.DOCKER_CONTENT_TRUST}" 
        DOCKER_CONTENT_TRUST_ROOT_PASSPHRASE = "${params.DOCKER_CONTENT_TRUST_ROOT_PASSPHRASE}"
        DOCKER_CONTENT_TRUST_REPOSITORY_PASSPHRASE = "${params.DOCKER_CONTENT_TRUST_REPOSITORY_PASSPHRASE}"
    }
    stages {
        stage('Image Build') {
            steps {
                sh "env | sort"
                sh "docker rmi ${params.DTR_IP}/engineering/docker-node-app:latest"
                sh "docker build -t ${params.DTR_IP}/engineering/docker-node-app ."
                sh "docker tag ${params.DTR_IP}/engineering/docker-node-app ${params.DTR_IP}/engineering/docker-node-app:1.${BUILD_NUMBER}"
            }
        }
        stage('DTR Push') {
            steps {
                sh "docker login -u ${params.UCP_USER} -p ${params.UCP_PASSWORD} ${params.DTR_IP}"
                sh "docker push ${params.DTR_IP}/engineering/docker-node-app"
                sh "docker push ${params.DTR_IP}/engineering/docker-node-app:1.${BUILD_NUMBER}"
            }
        }
        stage('App Deploy') {
            steps {
                sh "echo 'App Deploy'"
            }
        }
        stage('App Test') {
            steps {
                sh "echo 'App Test'"
            }
        }
    }
}
