pipeline {
    agent {
        kubernetes {
            label 'jenkins-jenkins-agent'
            defaultContainer 'dind'
        }
    }
    tools{
        jdk 'JDK 17'
    }
    environment {
        // DOCKER_NEXUS_HOST = "nexus:8082"
        // DOCKER_IMAGE = "jooeel98/application"
        // DOCKER_TAG = "0.4.0"
        SONAR_HOST_URL = 'https://sonar-default.ez-ibm-openshift-vpc-6zmu-b9be9ed6ae33d743815245d0b773ebc7-0000.eu-es.containers.appdomain.cloud/'
        SONAR_TOKEN = credentials('sonar-token')
    }
 
    stages {
        stage('Install Java 17') {
            steps {
                sh '''
                    apk update && apk add --no-cache openjdk17
                '''
            }
        }
        stage('Hola mundo'){
            steps{
                sh "java -version"
            }
        }
 
        stage('Checkout Code') {
            steps {
                //  Clona el repositorio explícitamente si no usas Multibranch Pipeline
                git branch: 'main', url: 'https://github.com/stemdo-labs/final-project-gestion-rrhh-frontend-jjfernandezz'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube Scanner'
                    withSonarQubeEnv('SonarQube') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner -X\
                            -Dsonar.projectKey=jenkins-security-project \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=${SONAR_HOST_URL} \
                            -Dsonar.login=${SONAR_TOKEN}
                        """
                    }
                }
            }
        }
    }
}