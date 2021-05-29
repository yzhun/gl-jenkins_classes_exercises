pipeline {
    agent { label "master" }
    
    environment {
        GREEN_BEGINNING="\033[32m"
        GREEN_END="\033[0m"
        BRANCH="feature/yzhun"
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
                wrap([$class: 'BuildUser']) {
                    // use name of the patchset as the build name
                    buildName "#${BUILD_NUMBER}_${BUILD_USER}_${BRANCH}"
                }
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
            cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'cover/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false
            archiveArtifacts("doc/source/*.rst, dist/*.whl")
        }
    }
}
