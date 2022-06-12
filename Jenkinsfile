pipeline{
    agent{
        label "docker"
    }
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
                archiveArtifacts artifacts: "results/*.xml"
            }
        }
        stage('API tests') {
            steps {
                sh "make test-api"
                archiveArtifacts artifacts: "results/*.xml"
            }
        }
        stage("E2E tests") {
            steps {
                sh "make test-e2e"
                archiveArtifacts artifacts: "results/*.xml"
            }
        }
    }
    post { 
        always { 
            junit "results/*_result.xml"
            cleanWs()
        }
        failure {  
            mail 
                subject: "ERROR CI", 
                to: "${EMAIL_TO}",
                body: "<b>Example</b>", 
                charset: 'UTF-8';
        }
    }
}

