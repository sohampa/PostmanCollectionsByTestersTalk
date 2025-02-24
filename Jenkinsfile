pipeline {
    agent { label 'soham-ec2' }

    stages {
        stage('Run API Tests') {
            steps {
                sh '''
                newman run "Postman Collections/PostmanCollectionByTestersTalk.json" \
                -e "Postman Collections/Booking API.postman_environment.json" \
                --reporters cli,junit \
                --reporter-junit-export results.xml
                '''
            }
        }

        stage('Publish Test Results') {
            steps {
                junit 'results.xml'
            }
        }
    }
}
