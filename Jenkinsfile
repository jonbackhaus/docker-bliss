pipeline {
  environment {
    registry = "romancin/bliss"
    registryCredential = 'dockerhub'
  }
  agent any
  stages {
    stage('Cloning Git Repository') {
      steps {
        git url: 'https://github.com/romancin/bliss-docker.git',
            branch: '$BRANCH_NAME'
      }
    }
    stage('Building image and pushing it to the registry') {
            steps {
                script {
                    def gitbranch = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
                    def version = readFile('VERSION')
                    def versions = version.split('\\.')
                    def major = versions[0]
                    def minor = versions[0] + '.' + versions[1]
                    def patch = version.trim()
                    docker.withRegistry('', registryCredential) {
                        def image = docker.build registry + ":latest"
                        image.push()
                        image.push(major)
                        image.push(minor)
                        image.push(patch)
                    }
                }
            }
    }
 }
 post {
        success {
            telegramSend '[Jenkins] - Pipeline CI-bliss-docker $BUILD_URL finalizado con estado :: $BUILD_STATUS'    
        }
    }
}