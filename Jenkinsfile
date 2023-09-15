pipeline {
    agent any

    tools {
        jdk "jdk-17"
        maven "maven-3"
    }

    stages {
       stage('Clean workspace') {
            steps {
                cleanWs disableDeferredWipeout: true, deleteDirs: true
            }
        }

        stage('Pull pet-clinic') {
            steps {
                 git branch: 'main', url: 'https://github.com/Szwaczyn/szkolenie-cicd-jenkins-gitlab-example.git'
            }
            post {
                failure {
                    slackSend message: 'Faild'
                }
            }
        }

        stage('Build') {
            steps {
                sh "mvn clean install -DskipTests"
                slackSend message: 'Budowa wersji ' + env.BUILD_TAG + ' link: ' + env.BUILD_URL
                slackSend message: "Wersja ${params.VERSION_NAME} na środowisku ${params.DEV_EVIROMENT}"
            }
        }

        stage('Tests') {
            steps {
                 sh "mvn test"
                 slackSend message: "Testy ok"
            }
        }
    }
}
