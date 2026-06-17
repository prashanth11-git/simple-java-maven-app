pipeline {
    agent any

    stages {
        stage('Pull Web Code') {
            steps {
                // This clones a REAL web application with HTML, CSS, and images
                git url: 'https://github.com/mdn/beginner-html-site-scripted.git', branch: 'main'
            }
        }

        stage('Publish Website on 9090') {
            steps {
                echo 'Clearing port 9090...'
                // Kills anything currently running on port 9090
                sh 'fuser -k 9090/tcp || true'
                
                echo 'Starting Web Server on port 9090...'
                // Launches a production-ready Python web server inside the web files directory
                sh 'JENKINS_NODE_COOKIE=dontKillMe nohup python3 -m http.server 9090 > webserver.log 2>&1 &'
                
                echo 'Website is now live!'
            }
        }
    }
}
