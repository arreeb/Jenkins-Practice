pipeline {
    // Forces the job to run specifically on your Slave node
    agent { label 'linux-worker' }

 

    // Ensures GitHub webhooks automatically trigger this pipeline
    triggers {
        githubPush()
    }

 

    stages {
        stage('Checkout Code') {
            steps {
                // Automatically pulls your code from GitHub into the Slave's workspace
                checkout scm
            }
        }

        stage('Build Application') {
            steps {
                // Replace this with your actual build command (e.g., mvn clean package or npm run build)
                echo "Compiling application..."
                sh "echo 'Simulating compilation...' > my-app-v${env.BUILD_ID}.jar"
            }
        }

 

        stage('Run Tests') {
            steps {
                // Without a parameter checkbox, we just run the tests automatically every time
                echo "Starting unit tests..."
                sh "echo 'Test Suite Completed Successfully!'"
            }
        }

 

        stage('Archive Artifacts') {
            steps {
                // Stores the physical output of your build safely on the Master node
                archiveArtifacts artifacts: '*.jar', allowEmptyArchive: true
            }
        }
    }

 

    // Handles the email alerts regardless of the build's outcome
    post {
        success {
            emailext (
                to: 'arreeb18@gmail.com',
                subject: "SUCCESS: Job '${env.JOB_NAME}' [${env.BUILD_NUMBER}]",
                body: """Great news!

                The build and tests completed successfully.

                You can download your build artifact here:
                ${env.BUILD_URL}artifact/

                View Console Output: ${env.BUILD_URL}""",
                attachLog: true
            )
        }
        failure {
            emailext (
                to: 'arreeb18@gmail.com',
                subject: "FAILED: Job '${env.JOB_NAME}' [${env.BUILD_NUMBER}]",
                body: "The build failed. Please check the attached logs to troubleshoot.",
                attachLog: true
            )
        }
    }
}
