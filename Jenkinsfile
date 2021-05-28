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

       stage("Run unit tests") {
            steps {
                echo "${GREEN_BEGINNING}Run unit tests${GREEN_END}\n"
                sh 'tox -e py27'
            }
        }
        stage("Run docs") {
            steps {
                echo "${GREEN_BEGINNING}Run docs build${GREEN_END}\n"
                sh 'tox -e docs'
            }
        }
        stage("Run coverage build") {
            steps {
                echo "${GREEN_BEGINNING}Run coverage build${GREEN_END}\n"
                sh 'tox -e cover'
            }
        }
        stage("Build python package") {
            steps {
                echo "${GREEN_BEGINNING}Build python package${GREEN_END}\n"
                sh 'python setup.py bdist_wheel'
            }
        }
    }
    
    post {
        success {
            echo "Finish of pipeline with status success."
            archiveArtifacts("doc/source/*.rst, dist/*.whl")
        }
    }
}
