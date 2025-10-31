pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo "Pulling code from Git..."
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies..."
                
                // For Node.js
                sh '''
                  if [ -f package.json ]; then
                     echo "Node project detected"
                     npm install
                  fi
                '''

                // For Python
                sh '''
                  if [ -f requirements.txt ]; then
                     echo "Python project detected"
                     pip install -r requirements.txt
                  fi
                '''

                // For Maven (Java)
                sh '''
                  if [ -f pom.xml ]; then
                     echo "Maven project detected"
                     mvn install -DskipTests
                  fi
                '''
            }
        }

        stage('Build and Test') {
            steps {
                echo "Running build/test..."
                
                sh '''
                  if [ -f package.json ]; then
                     npm test || true
                     npm run build || true
                  fi

                  if [ -f pom.xml ]; then
                     mvn test
                  fi

                  if [ -f requirements.txt ]; then
                     pytest || true
                  fi
                '''
            }
        }

        stage('Archive Artifact') {
            steps {
                echo "Archiving build output..."
                
                archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
                archiveArtifacts artifacts: '**/dist/**', allowEmptyArchive: true
                archiveArtifacts artifacts: '**/build/**', allowEmptyArchive: true
            }
        }
    }
}
