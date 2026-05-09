pipeline {

    agent { label 'linux-worker' }

    triggers {
        githubPush()
    }

    environment {
        APP_NAME = "demo-app"
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Pulling latest code from GitHub..."
                checkout scm
            }
        }

        stage('Build Application') {
            steps {
                echo "Building application..."

                sh """
                    echo "Build started at \$(date)" > ${APP_NAME}-${BUILD_NUMBER}.jar
                    echo "Application compiled successfully" >> ${APP_NAME}-${BUILD_NUMBER}.jar
                """
            }
        }

        stage('Run Tests') {
            steps {
                echo "Running test cases..."

                sh """
                    echo "Executing unit tests..."
                    echo "All tests passed successfully"
                """
            }
        }

        stage('Archive Artifact') {
            steps {
                echo "Archiving build artifact..."

                archiveArtifacts artifacts: '*.jar', fingerprint: true
            }
        }
    }

    post {

        success {
            echo "Build completed successfully!"

            emailext(
                to: 'arreeb18@gmail.com',
                subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
Build completed successfully.

Job Name: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}

Artifact Link:
${env.BUILD_URL}artifact/

Console Output:
${env.BUILD_URL}console
""",
                attachLog: true
            )
        }

        failure {
            echo "Build failed!"

            emailext(
                to: 'arreeb18@gmail.com',
                subject: "FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
Build failed.

Check Console Output:
${env.BUILD_URL}console
""",
                attachLog: true
            )
        }
    }
}
