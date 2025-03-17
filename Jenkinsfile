pipeline {
    agent any

    tools {
        maven 'MAVEN_HOME'
    }

    stages {
        // Stage to build on pull request (PR) branches
        stage('Build PR') {
            when {
                branch pattern: "PR-.*", comparator: 'REGEXP'
            }
            steps {
                script {
                    echo "Building PR branch"
                    bat 'mv clean install'
                }
            }
        }

        // Stage to build feature and develop branches
        stage('Build Feature or Develop') {
            when {
                branch 'feature/*' // Replace with your feature branch pattern or develop if you have one
            }
            steps {
                script {
                    echo "Building branch"
                    bat 'mv clean install'
                }
            }
        }

        // Stage to build main branch (this will also send the email)
        stage('Build Main') {
            when {
                branch 'main'
            }
            steps {
                script {
                    echo "Building main branch"
                    bat 'mv clean package'
                    // Assuming that the Maven build produces a jar file in target/
                    def jarFile = findFiles(glob: 'target/*.jar')[0]

                    // Send an email notification after successful build
                    emailext (
                        to: "devduku@gmail.com",
                        subject: "Build Successful for Main Branch",
                        body: "The main branch build was successful and the jar is ready. The file is located at ${jarFile}",
                        attachmentsPattern: jarFile,
                        mimeType: 'text/html'
                    )
                }
            }
        }

    }

    post {
        always {
            echo "Cleaning up after build..."
            cleanWs() // Clean up workspace after each run
        }

        success {
            echo "Build successful!"
        }

        failure {
            echo "Build failed. Please check the logs."
        }
    }
}
