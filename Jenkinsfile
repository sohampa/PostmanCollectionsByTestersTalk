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
                sh '''
                newman run "Postman Collections/PostmanCollectionByTestersTalk.json" \
                -e "Postman Collections/Booking API.postman_environment.json" \
                --reporters cli,junit,htmlextra \
                --reporter-junit-export results.xml \
                --reporter-htmlextra-export newman/html-report.html
                '''
            }
        }
 
        stage('Publish Test Results') {
            steps {
                junit 'results.xml'
            }
        }

        stage('Publish HTML Report') {
            steps {
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

    // post {
    //     always {
    //         echo 'Checking if the newman directory exists...'
    //         sh 'chmod -R 755 newman'
    //         sh 'chown -R jenkins:jenkins newman || echo "Ignoring ownership change"'
    //         sh 'ls -lah newman || echo "Newman directory not found!"'
            
    //         echo 'Publishing HTML Report...'
    //         publishHTML(target: [
    //             allowMissing: false,
    //             alwaysLinkToLastBuild: true,
    //             keepAll: true,
    //             reportDir: 'newman',
    //             reportFiles: 'api-test-report.html',
    //             reportName: 'API Test Report'
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
