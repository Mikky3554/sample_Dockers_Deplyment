pipeline {
    agent any
    tools {
        maven 'Maven'
    }
 stages {
        stage('checkout') {
                steps {
                    git branch: 'main', url: 'https://github.com/Mikky3554/samplek8s.git'
                }
           }
        stage('Maven Build') {
           steps {
                sh 'mvn clean install -Dmaven.test.skip -DskipTests'             
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv(installationName: 'SonarQubeDev') {
                sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        stage('Build Image & Deploy'){
            steps {
                sh 'sudo docker build -t mikky3554/japp:LATEST .'
                //sh 'sudo docker tag japp mikky3554/japp:$BUILD_NUMBER'
                sh 'sudo docker images'
                sh 'sudo docker stop japp_cont;'
                sh 'sudo docker rm -f japp_cont;'
                sh 'sudo docker run -d --name japp_cont -p 8090:8083 mikky3554/japp:LATEST'
                sh 'sudo docker ps'
            }
        }
    }    
}    
