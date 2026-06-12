pipeline {
    agent any

    stages {

        stage('Detect Changes') {
            steps {
                script {

                    def changedFiles = bat(
                        script: '@git diff --name-only HEAD~1 HEAD',
                        returnStdout: true
                    ).trim()

                    echo "Changed Files:"
                    echo changedFiles

                    if (changedFiles.contains("gcc-config/")) {

                        echo "GCC Environment Selected"

                    } else if (changedFiles.contains("mini-dev-config/")) {

                        echo "Mini Dev Environment Selected"

                    } else {

                        echo "Default Environment Selected"
                    }
                }
            }
        }
    }
}
