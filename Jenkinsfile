// The 'pipeline' block is the top-level block for a Declarative Pipeline.
// All other directives (agent, stages, post, etc.) must be inside this block.
pipeline {
    // Defines the agent where the pipeline will run. 'any' means Jenkins will pick any available agent.
    // You might specify a label like 'agent { label 'my-gradle-agent' }' if you have specific build agents.
    agent any

    // Use the 'tools' block to define which tools should be made available on the agent.
    // The name 'Gradle_9.1' must match the name you configured in Jenkins Global Tool Configuration.
    tools {
        gradle 'Gradle_9.1' // IMPORTANT: Replace 'Gradle_9.1' with the name you set in Jenkins
    }

    // The 'stages' block defines a series of logical steps in your CI/CD process.
    stages {
        // Stage for cloning the repository, though Jenkins usually handles this automatically for Pipeline from SCM.
        stage('Checkout') {
            steps {
                // This step is often implicit when using "Pipeline script from SCM" in Jenkins job configuration,
                // but explicitly calling 'checkout scm' ensures the latest code is pulled.
                checkout scm
            }
        }

        // Stage for building the Gradle project.
        stage('Build') {
            steps {
                // Use the 'sh' (shell) step to execute shell commands.
                // Since Gradle is now defined in the 'tools' section, Jenkins will add it to the PATH.
                sh 'gradle clean build'
            }
        }

        // Optional: Stage for running tests (if you add them to your project).
        // For this simple project, we don't have tests yet, but this is where they'd go.
        stage('Test') {
            steps {
                // Example for running tests (uncomment if you add tests later)
                // sh 'gradle test'
                echo 'Skipping tests for this simple example.' // Placeholder for now
            }
        }

        // Optional: Stage for archiving build artifacts (like the generated JAR).
        stage('Archive Artifacts') {
            steps {
                // 'archiveArtifacts' stores the specified files so they can be downloaded from Jenkins.
                // Adjust the path to match where your JAR file is generated (usually build/libs/).
                archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
            }
        }
    }

    // The 'post' section defines actions that run after the main pipeline stages complete,
    // regardless of success or failure.
    post {
        always {
            // 'cleanWs' cleans up the workspace on the agent after the build.
            // Ensure Workspace Cleanup Plugin is installed.
            cleanWs()
        }
        success {
            echo 'Pipeline finished successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}

