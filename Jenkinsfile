pipeline {
    agent any

    tools {
        maven 'Maven 3.x' 
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/jenkins-docs/simple-java-maven-app.git', branch: 'master'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Publish / Run Website') {
            steps {
                echo 'Stopping any existing instance on port 9090...'
                sh 'fuser -k 9090/tcp || true' 
                
                echo 'Starting the application via Maven Jetty Plugin on port 9090...'
                // Using mvn jetty:run ensures the web server actually starts up and listens on 9090
                sh 'JENKINS_NODE_COOKIE=dontKillMe nohup mvn jetty:run -Djetty.http.port=9090 > app.log 2>&1 &'
                
                echo 'Waiting 5 seconds for server to spin up...'
                sh 'sleep 5'
            }
        }
    }
}
