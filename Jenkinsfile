pipeline {
    agent { label 'soham-ec2' }
    // tools {
    //     nodejs 'NodeJS' // Use Node.js plugin
    // }

    // environment {
    //     PATH = "${tool 'NodeJS'}/bin:${env.PATH}" // Add Node.js to PATH
    // }
    environment {
        POSTMAN_CLI_PATH = "/usr/local/bin/postman" // Adjust path if needed
    }

    stages {

         stage('Install Postman CLI') {
            steps {
                script {
                    sh '''
                    if ! command -v postman &> /dev/null; then
                        echo "Postman CLI not found. Installing..."
                        curl -o- "https://dl-cli.pstmn.io/install/linux64.sh" | sh
                    else
                        echo "Postman CLI is already installed."
                    fi
                    '''
                }
            }
        }

         stage('Run API Tests') {
             steps {
                 script {
                     catchError(buildResult: 'UNSTABLE', stageResult: 'UNSTABLE') {
                         sh '''
                         postman collection run "Postman Collections/PostmanCollectionByTestersTalk.json" \
                         -e "Postman Collections/Booking API.postman_environment.json" \
                         --reporters junit,html --reporter-junit-export results.xml --reporter-html-export postman-report/result.html
                         '''
                        }
                    }
                }
             post {
                always {
                    echo "Publishing Postman HTML Report..."
                    publishHTML([
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'postman-report',
                        reportFiles: 'result.html',
                        reportName: 'Postman API Test Report'
                    ])
                }
        }
                 
            }

    //     stage('Run API Tests') {
    //         steps {
    //             script {
    //                 // Run Newman and ensure the build result is always SUCCESS
    //                 catchError(buildResult: 'UNSTABLE', stageResult: 'UNSTABLE') {
    //                     sh '''
    //                     newman run "Postman Collections/PostmanCollectionByTestersTalk.json" \
    //                     -e "Postman Collections/Booking API.postman_environment.json" \
    //                     -r htmlextra --reporter-htmlextra-export newman/html-report.html
    //                     '''
    //                 }
    //             }
    //         }
    //         post {
    //             always {
    //                 // Publish the HTML report
    //                 publishHTML (target: [
    //                     allowMissing: false,
    //                     alwaysLinkToLastBuild: true,
    //                     keepAll: true,
    //                     reportDir: 'newman',
    //                     reportFiles: 'html-report.html',
    //                     reportName: 'Newman HTML Report'
    //                 ])
    //             }
    // }
        // }

    // }

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
}
