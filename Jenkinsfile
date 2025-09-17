// This is a Declarative Pipeline script
pipeline {
    // 1. Specify the build environment
    agent any

    // 2. Define tools to be auto-installed
    tools {
        // This makes the NodeJS tool we configured earlier available
        nodejs 'NodeJS-LTS'
    }

    // 3. Define the stages of our pipeline
    stages {
        // The 'Build' stage
        stage('Build') {
            steps {
                echo '--- INSTALLING DEPENDENCIES (BUILD STEP) ---'
                // The 'sh' step runs a shell command
                sh 'npm install'
            }
        }

        // The 'Test' stage
        stage('Test') {
            steps {
                echo '--- RUNNING TESTS ---'
                sh 'npm test'
            }
        }
    }

    // 4. Define actions to take after the pipeline runs
    post {
        // This 'success' block only runs if all stages passed
        success {
            echo 'Pipeline was successful! Archiving artifacts.'
            // This archives our application code
            archiveArtifacts artifacts: '**/*', followSymlinks: false
        }
    }
}
