pipeline  {
    agent {
        label "jenkins-agent"
    }

    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    environment{
        APP_NAME = "production-e2e-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "popeschmidt"
        IMAGE_NAME = "${DOCKER_USER}" + "/" +"${APP_NAME}"
        IMAGE_TAG = "${RELEASE}" + "-"+ "${env.BUILD_NUMBER}"
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
        
        stage("SonarQube Analysis"){
            steps{
                withSonarQubeEnv(installationName: "sonarqube-scanner", credentialsId: 'jenkins-sonarqube-token' ) {
                sh "mvn sonar:sonar"
                }
            }
        }

        stage("Quality Gate"){
            steps{
                waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                }
        }

        stage("Build and push Docker image"){
            steps{
                    script{
                        docker.withRegistry('', credentialsId: 'dockerhub'){
                        docker_image = docker.build "${IMAGE_NAME}"
                        docker_image.push("${IMAGE_TAG}")
                    }
                }
            }

        }

    }
}