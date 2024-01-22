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

                    // Check if the commit is associated with a tag
                    def isTaggedCommit = sh(script: 'git describe --exact-match HEAD', returnStatus: true) == 0

                    if (isTaggedCommit) {
                        // Extract the version information from the tag
                        def version = sh(script: 'git describe --tags --abbrev=0', returnStdout: true).trim()

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
                            sh 'docker run -p 5000:5000 myflaskapp'
                            // Your build steps go here
                        } else if (patch == '1') {
                            // Exit the pipeline with a message if the last digit is 1
                            error 'Last digit is 1. Exiting without build.'
                        } else {
                            // Handle other cases if needed
                            echo 'Custom logic for other cases...'
                        }
                    } else {
                        // Handle cases where the commit is not associated with a tag
                        echo 'Commit is not associated with a tag. Skipping version check.'
                    }
                }
            }
        }
    }
}
