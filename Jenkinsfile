pipeline {
agent any

```
environment {
    TARGET_REPO = ""
}

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

                    env.TARGET_REPO = "gcc-mirror-repo"
                    echo "GCC Environment Selected"

                } else if (changedFiles.contains("mini-dev-config/")) {

                    env.TARGET_REPO = "mini-dev-mirror-repo"
                    echo "Mini Dev Environment Selected"

                } else {

                    echo "Default Environment Selected"
                }
            }
        }
    }

    stage('Mirror Repository') {

        when {
            expression {
                return env.TARGET_REPO?.trim()
            }
        }

        steps {

            withCredentials([
                usernamePassword(
                    credentialsId: 'github-creds',
                    usernameVariable: 'GIT_USER',
                    passwordVariable: 'GIT_TOKEN'
                )
            ]) {

                bat """
                git remote remove mirror 2>NUL

                git remote add mirror https://%GIT_USER%:%GIT_TOKEN%@github.com/shlokdobriyal/%TARGET_REPO%.git

                git push --mirror mirror
                """
            }
        }
    }
}
```

}
