
pipeline {
    agent any

    parameters {
        choice(
            name: 'TEST_RESULT',
            choices: ['SUCCESS', 'FAILURE'],
            description: 'Choose a result to test email notifications'
        )
    }

    environment {
        NOTIFY_EMAIL = 'toqaosama86@gmail.com'
    }

    stages {
        stage('Start') {
            steps {
                script {
                    env.PIPELINE_START =
                        new Date().format(
                            'yyyy-MM-dd HH:mm:ss z',
                            TimeZone.getTimeZone('Africa/Cairo')
                        )
                }

                echo "Pipeline started at: ${env.PIPELINE_START}"
            }
        }

        stage('Build') {
            steps {
                echo 'Building application...'
                sh 'echo Build completed'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing application...'

                script {
                    if (params.TEST_RESULT == 'FAILURE') {
                        error('Intentional failure for testing')
                    }
                }

                sh 'echo Tests passed'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh 'echo Deployment completed'
            }
        }
    }

    post {
        success {
            script {
                emailext(
                    to: env.NOTIFY_EMAIL,
                    subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    body: """
Hello,

Your Jenkins pipeline completed successfully.

Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
Status: SUCCESS
Started: ${env.PIPELINE_START}
Finished: ${new Date().format('yyyy-MM-dd HH:mm:ss z', TimeZone.getTimeZone('Africa/Cairo'))}
Duration: ${currentBuild.durationString}

Build URL: ${env.BUILD_URL}
"""
                )
            }
        }

        failure {
            emailext(
                to: env.NOTIFY_EMAIL,
                subject: "FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
Hello,

Your Jenkins pipeline has FAILED.

Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
Status: FAILURE
Started: ${env.PIPELINE_START}
Finished: ${new Date().format('yyyy-MM-dd HH:mm:ss z', TimeZone.getTimeZone('Africa/Cairo'))}
Duration: ${currentBuild.durationString}

Please check the Console Output:
${env.BUILD_URL}console
"""
            )
        }

        aborted {
            emailext(
                to: env.NOTIFY_EMAIL,
                subject: "ABORTED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
Hello,

Your Jenkins pipeline was ABORTED or cancelled.

Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
Status: ABORTED
Started: ${env.PIPELINE_START}
Aborted/finished: ${new Date().format('yyyy-MM-dd HH:mm:ss z', TimeZone.getTimeZone('Africa/Cairo'))}
Duration: ${currentBuild.durationString}

Build URL: ${env.BUILD_URL}
"""
            )
        }

        always {
            echo "Final status: ${currentBuild.currentResult}"
            echo "Pipeline started at: ${env.PIPELINE_START}"
            echo "Pipeline duration: ${currentBuild.durationString}"
        }
    }
}
