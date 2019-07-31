node ('master') {
    stage ('Checkout') {
        checkoutSubmodule()
    }

    stage ('Pre-commit Checks') {
        REPO_NAME = env.JOB_NAME.split('/')[1]
        PKG_NAME  = REPO_NAME.substring(0, REPO_NAME.length() - 4)
        dir(PKG_NAME) {
            preCommit()
        }
    }

    stage ('Build') {
        buildGenericPkg()
    }
    stage ('Test') {
        // runPythonUnitTesting(env.WORKSPACE + "/pt-update/tests/", env.WORKSPACE + "/pt-update/src/")
        checkSymLinks()
        shellcheck()
        try {
            lintian()
        } catch (e) {
            currentBuild.result = 'UNSTABLE'
        }
    }

    stage ('Publish') {
        publishSirius()
    }
}