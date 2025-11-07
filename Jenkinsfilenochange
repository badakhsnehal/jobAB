pipeline {
    agent { label 'builtinubuntu' } // Use the Ubuntu agent

    tools {
        maven 'maven 3.9.10'          // Maven from global tool configuration
        jdk 'openjdk 21.0.7'          // JDK from global tool configuration
    }

    stages {
        stage('Fetch Code') {
            steps {
                git branch: 'main', url: 'https://github.com/hkhcoder/vprofile-project.git'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'mvn test'  // Compile and run unit tests
            }
        }

        stage('Build Code') {
            steps {
                sh 'mvn package -DskipTests'  // Package without running tests
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: '**/*.war', allowEmptyArchive: true
            }
        }

        stage('Deploy to Staging') {
            when {
                expression { currentBuild.currentResult == 'SUCCESS' } // Only run if previous stages succeeded
            }
            steps {
                echo 'Deploying artifact to staging server.'
            }
        }
    }

    post {
        failure {
            sh 'echo "Build failed. Notifying the development team."' // Notify on failure
        }
    }
}
