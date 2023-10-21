pipeline {

    agent any

    environment {
        MYSQL_ROOT_LOGIN = credentials('my-sql-account')
        IMAGE_NAME = 'nodejs'
        TIMESTAMP = sh(returnStdout: true, script: "date +%d.%m.%Y").trim()
        TAG = "${TIMESTAMP}-v${BUILD_NUMBER}"
    }
    stages {
        stage ('Define TAG') {
            steps {
                script {
                	echo "TAG: $TAG"
                    writeFile file: 'tag.txt', text: "$TAG"
                }
            }
        }
        stage('Packaging/Pushing image') {
            steps {
                withDockerRegistry(credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/') {
                    sh 'docker build -t donhatanh2000/nodejs:$TAG .'
                    sh 'docker tag donhatanh2000/nodejs:${TAG} donhatanh2000/nodejs:latest'
                    sh 'docker push donhatanh2000/nodejs:${TAG}'
                    sh 'docker push donhatanh2000/nodejs:latest'
                }
            }
        }
    }

    post {
        // Clean after build
        always {
            cleanWs()
        }
    }
}
