pipeline{
    agent{
        label "jenkins-agent"
    }
    tools {
        jdk 'java17'
        maven 'maven3'
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
        stage("Quality gate"){
            steps{
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube-token'
                }
                
            }
           
        }
        

    }
   
}