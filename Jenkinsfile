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
                cleanWS()
            }
           
        }

        stage("Git Checkout"){
            steps{
                git branch: 'main', credentialsId: 'github-cred', url: 'https://github.com/Ankit1680/complete-prodcution-e2e-pipeline'
            }
           
        }


    }
   
}