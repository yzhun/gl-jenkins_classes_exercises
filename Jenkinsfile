pipeline {
    agent { label "master" }

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