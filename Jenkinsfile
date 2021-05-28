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

       stage("1 Run unit tests") {
            steps {
                echo -e "${GREEN_BEGINNING}Run unit tests${GREEN_END}\n"
                sh 'ox -e py27'
            }
        }
        stage("2 Run docs") {
            steps {
                echo -e "${GREEN_BEGINNING}Run docs build${GREEN_END}\n"
                sh 'tox -e docs'
            }
        }
        stage("3 Run coverage build") {
            steps {
                echo -e "${GREEN_BEGINNING}Run coverage build${GREEN_END}\n"
                sh 'tox -e cover'
            }
        }
        stage("4 Build python package") {
            steps {
                echo -e "${GREEN_BEGINNING}Build python package${GREEN_END}\n"
                sh 'python setup.py bdist_wheel'
            }
        }

    post {
        success {
            echo "Finish of pipeline with status success."
            archiveArtifacts("doc/source/*.rst, dist/*.whl")
        }
    }
}
