pipeline {
    agent any

    tools {
        maven '3.9.9'
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
                    sh 'mvn clean install'
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
                    sh 'mvn clean install'
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
                    sh 'mvn clean package'
                    archiveArtifacts artifacts: 'target/*.jar, target/*.war'


                        // Send email with the jar file attached
                        emailext(
                            to: "devduku@gmail.com",
                            subject: "Build Successful for Main Branch",
                            body: "The main branch build was successful, and the jar is ready.",
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
