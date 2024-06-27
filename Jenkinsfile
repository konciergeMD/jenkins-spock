@Library(['v1-jenkins-pipeline-library', 'v1-jenkins-pipeline-library-java']) _ //importing pipeline libraries

try {
    podTemplate(yaml: readTrusted('podTemplates/lib-test-pod.yaml')) {
        node(POD_LABEL) {
            stage('Checkout code') {
                checkout scm
                common.setupRepoName()
            }

            if (env.BRANCH_NAME == 'master') {
                mavenBuildAndDeploy(skipDeploy: false, skipPackage: false)

            } else {
                mavenBuildAndDeploy(skipDeploy: true, skipPackage: false)
            }
        }
    }
    currentBuild.result = 'SUCCESS'
} catch (Exception e) {
    currentBuild.result = 'FAILURE'
    throw e
} finally {
    slackSendMessage(onSuccess: false, onFailure: true, onStatusChange: true)
}
