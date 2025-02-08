pipeline{
    agent{
        label "jenkins-agent"
    }
    tools {
        jdk 'java17'
        maven 'maven3'
    }

    environment {
        APP_NAME = "complete-prodcution-e2e-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "ankit2849"
        DOCKER_PASS = credentials('docker-cred')  // Use Jenkins credentials
        IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${env.BUILD_NUMBER}"
    }
    stages{
        stage("Cleanup workspace"){
            steps{
                cleanWs()
            }
           
        }

        stage("Git Checkout"){
            steps{
                git branch: 'main', credentialsId: 'github-cred', url: 'https://github.com/Ankit1680/complete-prodcution-e2e-pipeline'
            }
           
        }
        stage("Build"){
            steps{
                sh "mvn clean package"
            }
           
        }
        stage("Test"){
            steps{
                sh "mvn test"
            }
           
        }
        stage("Sonarqube Analysis"){
            steps{
                script {
                    withSonarQubeEnv(credentialsId: 'sonarqube-token') {
                    sh "mvn sonar:sonar"
                    }
                }
                
            }
           
        }
        // stage("Quality gate"){
        //     steps{
        //         script {
        //             waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube-token'
        //         }
                
        //     }
           
        // }
        stage("Docker Image Build and Tag"){
            steps{
                script {
                    docker.withRegistry('', DOCKER_PASS){
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('', DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
                
            }
           
        }
        

    }
   
}