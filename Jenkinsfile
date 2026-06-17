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
                // Compiles the Java code and creates the test reports
                sh 'mvn clean package'
            }
        }

        stage('Publish Website') {
            steps {
                echo 'Stopping any existing instance on port 9090...'
                sh 'fuser -k 9090/tcp || true' 
                
                echo 'Creating a custom homepage for your deployment...'
                // This creates a nice web dashboard showing your app successfully ran
                sh '''
                    mkdir -p site-root
                    echo "<html><head><title>Java App Deployment</title></head><body style='font-family:sans-serif; text-align:center; margin-top:100px;'>" > site-root/index.html
                    echo "<h1>🚀 Jenkins Automation Success!</h1>" >> site-root/index.html
                    echo "<p>Your Simple Java Maven Application was successfully built.</p>" >> site-root/index.html
                    echo "<p><b>Build Time:</b> $(date)</p>" >> site-root/index.html
                    echo "</body></html>" >> site-root/index.html
                '''
                
                echo 'Launching web server on port 9090...'
                // Uses Python's built-in production-ready web module to host the site-root directory on 9090
                sh 'cd site-root && JENKINS_NODE_COOKIE=dontKillMe nohup python3 -m http.server 9090 > ../app.log 2>&1 &'
                
                echo 'Waiting 3 seconds for server spin up...'
                sh 'sleep 3'
            }
        }
    }
}
