pipeline {
    agent { label 'soham-ec2' }

    stages {

        // stage('Run API Tests') {
        //     steps {
        //         script {
        //             def exitCode = sh(
        //                 script: '''
        //                 postman collection run "Postman Collections/PostmanCollectionByTestersTalk.json" \
        //                 -e "Postman Collections/Booking API.postman_environment.json" \
        //                 --reporters junit --reporter-junit-export results.xml
        //                 ''',
        //                 returnStatus: true
        //             )
                    
        //             if (exitCode != 0) {
        //                 echo "Postman CLI tests failed, but proceeding to publish reports."
        //             }
        //         }
        //     }
        // }
        stage('Run API Tests') {
            steps {
                script {
                    // Run Newman and allow failures without stopping pipeline
                    def exitCode = sh(
                        script: '''
                        newman run "Postman Collections/PostmanCollectionByTestersTalk.json" \
                        -e "Postman Collections/Booking API.postman_environment.json" \
                        -r htmlextra \
                        --reporters cli,junit,html \
                        --reporter-junit-export results.xml \
                        --reporter-html-export newman/api-test-report.html
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
        always {
            echo 'Publishing HTML Report...'
            publishHTML(target: [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'newman',
                reportFiles: 'api-test-report.html',
                reportName: 'API Test Report'
            ])
        }
    }
    // post {
    //     always { // Ensure this runs regardless of success or failure
    //         echo "Publishing test results..."
    //         junit 'results.xml'
    //     }
    // }
}
