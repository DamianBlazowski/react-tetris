pipeline {
    agent any

    environment {
        MAJOR_VERSION = "1"
        MINOR_VERSION = "1"
        BUILD_VERSION = sh(returnStdout: true, script: 'echo ${BUILD_NUMBER}').trim()
        VERSION = "${MAJOR_VERSION}.${MINOR_VERSION}.${BUILD_VERSION}"
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }

    stages {
        stage('Build') {
            steps {
                sh 'yarn'
                sh 'yarn build'
            }
            post {
                always {
                    archiveArtifacts(artifacts: 'devops/*.txt', fingerprint: true, followSymlinks: false)
                }
                success {
                    echo 'Build success'
                }
                failure {
                    echo 'Build failure'             
                }
            }
        }

        stage('Test') {
            steps {
                sh 'yarn test'
            }
            post {
                always {
                    archiveArtifacts(artifacts: 'devops/*.txt', fingerprint: true, followSymlinks: false)
                }
                success {
                    echo 'Test success'
                }
                failure {
                    echo 'Test failure'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker-compose up -d'
            }
            post {
                always {
                    archiveArtifacts(artifacts: 'devops/*.txt', fingerprint: true, followSymlinks: false)
                }
                success {
                    echo 'Deployment success'
                }
                failure {
                    echo 'Deployment failure'
                }
            }
        }
    }
}
