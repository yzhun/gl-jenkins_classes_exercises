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
