pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                cleanWs() 
                echo 'Cloning code from GitHub...'
                git branch: 'main', url: 'https://github.com/Amiramuhammed/streamlit-ml-demo.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the Docker image...'
                sh 'docker build -t streamlit-ml-app:${BUILD_NUMBER} .'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing the newly built Docker image...'
                sh 'docker run --rm streamlit-ml-app:${BUILD_NUMBER} sh -c "python -m compileall ."'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Removing old container and deploying new one...'
                sh 'docker rm -f running-streamlit-app || true'
                sh 'docker run -d -p 8501:8501 --name running-streamlit-app streamlit-ml-app:${BUILD_NUMBER}'
            }
        }
    }
}