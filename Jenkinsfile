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
         //                 --reporters junit --reporter-junit-export results.xml \
         //                 --reporter-html-export newman/html-report.html 
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
                    // Run Newman and ensure the build result is always SUCCESS
                    catchError(buildResult: 'UNSTABLE', stageResult: 'UNSTABLE') {
                        sh '''
                        newman run "Postman Collections/PostmanCollectionByTestersTalk.json" \
                        -e "Postman Collections/Booking API.postman_environment.json" \
                        -r htmlextra --reporter-htmlextra-export newman/html-report.html
                        '''
                    }
                }
            }
            post {
                always {
                    // Publish the HTML report
                    publishHTML (target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'newman',
                        reportFiles: 'html-report.html',
                        reportName: 'Newman HTML Report'
                    ])
                }
    }
        }

    }

    // post {
    //     always {
    //         // Publish the HTML report
    //         publishHTML (target: [
    //             allowMissing: false,
    //             alwaysLinkToLastBuild: true,
    //             keepAll: true,
    //             reportDir: 'newman',
    //             reportFiles: 'html-report.html',
    //             reportName: 'Newman HTML Report'
    //         ])
    //     }
    // }
    // post {
    //     always { // Ensure this runs regardless of success or failure
    //         echo "Publishing test results..."
    //         junit 'results.xml'
    //     }
    // }
}
