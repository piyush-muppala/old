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
                    // Get latest commit message
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
                            sh 'docker-compose build'

                            sh 'docker-compose down && docker-compose up -d'
                        } else {
                            // minor versions
                            error "Minor version detected (last digit is not 0). Exiting without build."
                        }
                    } else {
                        echo 'Commit is not associated with a tag. Skipping version check, Skipping build'
                    }
                }
            }
        }
    }
}
