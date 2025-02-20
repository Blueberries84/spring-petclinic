pipeline {
    agent any

    tools {
        // Install the JDK and Maven
        jdk 'JDK 11'
        maven 'Maven 3.6.3'
    }

    environment {
        // Define environment variables
        JACOCO_HOME = tool name: 'Jacoco'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }
        stage('Build') {
            steps {
                // Build the project
                sh 'mvn clean install'
            }
        }
        stage('Test and Code Coverage') {
            steps {
                // Run the tests and generate the Jacoco code coverage report
                sh 'mvn test'
                sh 'mvn jacoco:report'
            }
            post {
                always {
                    // Publish the Jacoco code coverage report
                    jacoco(execPattern: '**/target/*.exec', 
                           classPattern: 'target/classes', 
                           sourcePattern: 'src/main/java', 
                           exclusionPattern: 'src/test*')
                }
            }
        }
    }

    triggers {
        // Schedule the pipeline to run every 10 minutes on Mondays
        cron('H/10 * * * 1')
    }
}
