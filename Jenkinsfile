pipeline{

    agent any

    environment {
        EMAIL_TO = 'jgomez.hrdz@gmail.com'
    }
    stages{
        stage('Source') {
            steps {
                git 'https://github.com/srayuso/unir-cicd.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building stage!'
                sh 'make build'
            }
        }
        stage('Unit tests') {
            steps {
                sh 'make test-unit'
                archiveArtifacts artifacts: "results/*.xml, coverage/*.html", allowEmptyArchive: true
            }
        }
        stage('API tests') {
            steps {
                sh "make test-api"
                archiveArtifacts artifacts: "results/*.xml, coverage/*.html", allowEmptyArchive: true
            }
        }
        stage("E2E tests") {
            steps {
                sh "make test-e2e"
                archiveArtifacts artifacts: "results/*.xml, coverage/**.html", allowEmptyArchive: true
            }
        }
    }
    post { 
        always { 
            junit "results/*_result.xml"
            publishHTML (target : [
                                    allowMissing: false,
                                    alwaysLinkToLastBuild: true,
                                    keepAll: true,
                                    reportDir: 'coverage',
                                    reportFiles: 'index.html',
                                    reportName: 'My Reports',
                                    reportTitles: 'The Report'
                                ])
            cleanWs()
        }
        failure {
            mail to: 'jgomez.hrdz@gmail.com', subject:"FAILURE in: ${env.JOB_NAME} execution number: ${currentBuild.number}", body: "Test Complete Build failed.";
        }
    }
}

