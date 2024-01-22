pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from Git
                    checkout scm
                }
            }
        }

        stage('Check Version and Build') {
            steps {
                script {
                    // Get the latest commit message
                    def commitMessage = sh(script: 'git log -1 --pretty=%B', returnStdout: true).trim()

                    // Define the version pattern directly
                    def versionPattern = /.*?(\d+)\.(\d+)\.(\d+).*/

                    // Extract version number from the commit message
                    def match = commitMessage =~ versionPattern

                    // Check if the version number is extracted successfully
                    if (match) {
                        def major = match.group(1)
                        def minor = match.group(2)
                        def patch = match.group(3)

                        echo "Major: ${major}, Minor: ${minor}, Patch: ${patch}"

                        // Check the last digit of the version
                        if (patch.toInteger() == 0) {
                            // Execute build if the last digit is 0
                            echo 'Executing build...'
                            sh 'docker build -t myflaskapp .'
                            sh 'docker run -p 5000:5000 myflaskapp'
                            // Your build steps go here
                        } else if (patch.toInteger() == 1) {
                            // Exit the pipeline with a message if the last digit is 1
                            error 'Last digit is 1. Exiting without build.'
                        } else {
                            // Handle other cases if needed
                            echo 'Custom logic for other cases...'
                        }
                    } else {
                        // Handle cases where version number is not found
                        echo 'Version number not found in commit message.'
                    }
                }
            }
        }
    }
}
