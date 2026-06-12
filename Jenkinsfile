def targetRepo = ""

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

                    targetRepo = "gcc-mirror-repo"
                    echo "GCC Environment Selected"

                } else if (changedFiles.contains("mini-dev-config/")) {

                    targetRepo = "mini-dev-mirror-repo"
                    echo "Mini Dev Environment Selected"

                } else {

                    targetRepo = ""
                    echo "Default Environment Selected"
                }
            }
        }
    }

    stage('Mirror Repository') {
        steps {
            script {

                echo "Target Repo = ${targetRepo}"

                if (targetRepo == "") {
                    echo "No mirroring required"
                    return
                }

                withCredentials([
                    usernamePassword(
                        credentialsId: 'github-creds',
                        usernameVariable: 'GIT_USER',
                        passwordVariable: 'GIT_TOKEN'
                    )
                ]) {

                    bat """
                    git remote remove mirror 2>NUL

                    git remote add mirror https://%GIT_USER%:%GIT_TOKEN%@github.com/shlokdobriyal/${targetRepo}.git

                    git push --mirror mirror
                    """
                }
            }
        }
    }
}

post {
    success {
        echo 'Pipeline completed successfully'
    }

    failure {
        echo 'Pipeline failed'
    }
}

}
