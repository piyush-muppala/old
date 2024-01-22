pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Check Version and Build') {
            steps {
                script {
                    // Get the latest commit message
                    def commitMessage = sh(script: 'git log -1 --pretty=%B', returnStdout: true).trim()

                    // Check if the commit message contains the version information
                    if (commitMessage.contains("update version to")) {
                        // Extract the version information
                        def version = commitMessage.tokenize(" ")[-1]

                        // Split the version into major, minor, and patch
                        def versionParts = version.split('\\.')

                        def major = versionParts[0] as String
                        def minor = versionParts[1] as String
                        def patch = versionParts[2] as String

                        echo "Major: ${major}, Minor: ${minor}, Patch: ${patch}"

                        // Check the last digit of the version
                        if (patch == '0') {
                            // Execute build if the last digit is 0
                            echo 'Executing build...'

                            // Run Docker commands outside the script block
                            sh 'docker build -t myflaskapp .'
                            sh 'docker run -p 5000:5000 -d myflaskapp'
                            // Your build steps go here
                        } else if (patch == '1') {
                            // Exit the pipeline with a message if the last digit is 1
                            error 'Last digit is 1. Exiting without build.'
                        } else {
                            // Handle other cases if needed
                            echo 'Custom logic for other cases...'
                        }
                    } else {
                        // Handle cases where version information is not found
                        echo 'Version information not found in commit message.'
                    }
                }
            }
        }
    }
}
