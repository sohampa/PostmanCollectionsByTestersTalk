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
                    // Run Newman and allow failures
                    catchError(buildResult: 'UNSTABLE', stageResult: 'UNSTABLE') {
                        sh '''
                        newman run "Postman Collections/PostmanCollectionByTestersTalk.json" \
                        -e "Postman Collections/Booking API.postman_environment.json" \
                        --reporters cli,junit,htmlextra \
                        --reporter-junit-export results.xml \
                        --reporter-htmlextra-export html-report.html
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            // Ensure test results are published even if the build fails
            junit 'results.xml'

            // Ensure HTML report is always available
            publishHTML (target: [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                // reportDir: 'newman',
                reportFiles: 'html-report.html',
                reportName: 'Newman HTML Report'
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
