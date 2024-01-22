pipeline {
    agent any

    environment {
        // Define the version pattern to match
        VERSION_PATTERN = /(\d+)\.(\d+)\.(\d+)/
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git
                script {
                    checkout scm
                }
            }
        }

        stage('Check Version and Build') {
            steps {
                script {
                    // Get the latest commit message
                    def commitMessage = sh(script: 'git log -1 --pretty=%B', returnStdout: true).trim()

                    // Extract version number from the commit message
                    def versionMatch = commitMessage =~ VERSION_PATTERN

                    // Check if the version number matches the pattern
                    if (versionMatch) {
                        def major = versionMatch[0][1]
                        def minor = versionMatch[0][2]
                        def patch = versionMatch[0][3]

                        echo "Major: ${major}, Minor: ${minor}, Patch: ${patch}"

                        // Check the last digit of the version
                        if (patch == '0') {
                            // Execute build if the last digit is 0
                            echo 'Executing build...'
                            // Your build steps go here
                        } else if (patch == '1') {
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
