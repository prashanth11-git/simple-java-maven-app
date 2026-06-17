pipeline {
    agent any

    tools {
        // References the Maven setup we created in Step 1
        maven 'Maven 3.x' 
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clones your specific repository
                git url: 'https://github.com/jenkins-docs/simple-java-maven-app.git', branch: 'master'
            }
        }

        stage('Build & Test') {
            steps {
                // Compiles the Java code and creates the executable JAR file
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Port 9090') {
            steps {
                echo 'Clearing port 9090 if it is already in use...'
                // Shuts down any older version running on 9090 so the new build doesn't crash
                sh 'fuser -k 9090/tcp || true' 
                
                echo 'Launching the web application on port 9090...'
                // Starts the app in the background using port 9090 and logs output to app.log
                sh 'nohup java -Djetty.http.port=9090 -jar target/my-app-1.0-SNAPSHOT.jar > app.log 2>&1 &'
            }
        }
    }
}
