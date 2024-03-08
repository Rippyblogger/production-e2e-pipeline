pipeline  {
    agent {
        label "jenkins-agent"
    }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    stages {
        stage("Cleanup workspace") {
            steps {
                echo "=========Cleaning workspace========="
                cleanWs()
            }
        }
    }

    stages{
        stage("Checkout Code") {
            steps {
                git branch: 'main', credentialsId: 'github', url:'git@github.com:Rippyblogger/production-e2e-pipeline.git'
            }
        }
    }
}