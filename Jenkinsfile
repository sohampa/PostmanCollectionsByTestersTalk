pipeline {
    agent { label 'soham-ec2' }

    stages {
        stage('Run API Tests') {
            steps {
                script {
                    // Run Newman and allow failures without stopping pipeline
                    def exitCode = sh(
                        script: '''
                        newman run "Postman Collections/PostmanCollectionByTestersTalk.json" \
                        -e "Postman Collections/Booking API.postman_environment.json" \
                        --reporters cli,junit \
                        --reporter-junit-export results.xml
                        ''',
                        returnStatus: true // Capture exit code instead of failing pipeline
                    )
                    
                    if (exitCode != 0) {
                        echo "Newman tests failed, but proceeding to publish reports."
                    }
                }
            }
        }
    }

    post {
        always { // Ensure this runs regardless of success or failure
            echo "Publishing test results..."
            junit 'results.xml'
        }
    }
}
