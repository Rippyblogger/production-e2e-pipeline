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

        stage("Checkout Code") {
            steps {
                git branch: 'main', credentialsId: 'github', url:'https://github.com/Rippyblogger/production-e2e-pipeline'
            }
        }

        stage("Build app") {
            steps {
                sh 'mvn clean package'
            }
        }

        stage("Test App") {
            steps {
                sh 'mvn test package'
            }
        }

    }
}