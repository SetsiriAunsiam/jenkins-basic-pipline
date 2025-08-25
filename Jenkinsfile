pipeline {
    agent any
    parameters {
        booleanParam(name: 'RUN_DEPLOY', defaultValue: true, description: 'Should we deploy?')
        choice(name: 'ENV', choices: ['dev', 'staging', 'prod'], description: 'Select the environment to deploy to')
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
            }
        }
        stage('Test in Parallel') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        echo 'Running unit tests...'
                        sh 'sleep 5'
                    }
                }
                stage('Integration Tests') {
                    steps {
                        echo 'Running integration tests...'
                        sh 'sleep 5'
                    }
                }
            }
        }
        stage('simulate testing') {
            parallel {
                stage('Linux Tests') {
                    steps {
                        echo 'Running tests on Linux...'
                        sh 'sleep 5'
                    }
                }
                stage('Windows Tests') {
                    steps {
                        echo 'Running tests on Windows...'
                        sh 'sleep 5'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                sh 'echo "All tests passed!" > results.txt'
                archiveArtifacts artifacts: 'results.txt', fingerprint: true
            }
        }
        stage('Approval') {
            steps {
                input "Do you want to proceed with deployment?"
                timeout(time: 2, unit: 'minutes') {
                    input message: 'Approve deployment?', ok: 'Deploy'
                }
            }
        }
        stage('Deploy') {
            when {
                expression { return params.RUN_DEPLOY }
            }
            steps {
                echo 'Deploying application...'
                echo "Deploying to environment: ${params.ENV}"
            }
        }
    }
    post {
        success {
        echo '✅ Pipeline finished successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check logs!'
        }
        always {
            echo 'Pipeline completed (success or failure).'
        }
    }
}