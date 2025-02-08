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
                echo "========executing A========"
            }
           
        }


    }
   
}