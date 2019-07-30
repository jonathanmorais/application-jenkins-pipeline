MAJOR_VERSION = "2"
PROJECT_NAME = "api"
VERSION = (env.BRANCH_NAME != 'develop') ? "${env.BRANCH_NAME}-${MAJOR_VERSION}.${env.BUILD_ID}" : "${MAJOR_VERSION}.${env.BUILD_ID}"

node ('build') {
    try {
        def build_image = docker.build("api:latest", "-f ./api/Dockerfile .")
        build_image.push()
    
    } catch(error) {
        deleteDir()
        throw error
    }
}

def createGitTag() {
    withCredentials([sshUserPrivateKey(credentialsId: 'github', keyFileVariable: 'GITHUB_KEY')]) {
        sh 'echo ssh -i $GITHUB_KEY -l git -o StrictHostKeyChecking=no \\"\\$@\\" > run_ssh.sh'
        sh 'chmod +x run_ssh.sh'
        withEnv(['GIT_SSH=./run_ssh.sh']) {
            sh "git tag '${VERSION}'"
            sh "git push origin '${VERSION}'"
        }
    }
}

def getDockerRunCommand(imagePatternList) {
    for (imagePattern in imagePatternList) {
       sh """sed -i 's|<'"$imagePattern"'-image>|'"${fullImageName(imagePattern)}"'|g' Makefile"""
    }

    sh "cat Makefile"
}