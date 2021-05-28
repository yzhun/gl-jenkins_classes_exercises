pipeline {
    agent { label "master" }
    
    environment {
        GREEN_BEGINNING="\033[32m"
        GREEN_END="\033[0m"
    }

    options {
        buildDiscarder(logRotator(numToKeepStr:'10'))
        ansiColor('xterm')
        timestamps ()
    }
    
    stages {

        stage("Checkout") {
        
            steps {
                git(
                    url: "https://opendev.org/jjb/jenkins-job-builder.git"
                )
            }
        }

        stage("Test stage") {
            steps {
                echo "Just an echo something..."
            }
        }
    }

    post {
        success {
            echo "Finish of pipeline"
        }
    }
}
