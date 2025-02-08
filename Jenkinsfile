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
        // DOCKER_PASS = credentials('docker-cred')  // Use Jenkins credentials
        DOCKER_PASS = "docker-cred"
        IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${env.BUILD_NUMBER}"
        JENKINS_API_TOKEN = credentials('JENKINS_API_TOKEN')
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
        stage("Trigger CD Pipeline"){
            steps{
                script {
                     sh "curl -v -k --user root:${JENKINS_API_TOKEN} -X POST -H 'cache-control: no-cache' -H 'content-type: application/x-www-form-urlencoded' --data 'IMAGE_TAG=${IMAGE_TAG}' 'http://13.233.138.185:8080/job/gitops-complete-pipeline/buildWithParameters?token=gitops-token'"

                    }

                
                }
                
            }
           
        
    }
   
}