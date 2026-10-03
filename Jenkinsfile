pipeline {
    agent any

    environment {
        MONGO_URI = 'mongodb://localhost:27017/test_student_db'
        SECRET_KEY = 'jenkins-test-secret'
    }

    stages {

        stage('Build') {
            steps {
                echo 'Installing Python dependencies...'
                sh '''
                    python3 -m venv venv
                    venv/bin/pip install --upgrade pip
                    venv/bin/pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running pytest...'
                sh '''
                    venv/bin/pytest -v
                '''
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying Flask application to staging...'
                sh '''
                    rm -rf staging
                    mkdir -p staging
                    cp -r app.py templates requirements.txt start_flask.sh staging/
                    echo "Flask application deployed to staging directory."
                    ls -la staging/
                '''
            }
        }
    }

post {
    success {
        echo 'Pipeline completed successfully!'
        emailext(
            subject: "Jenkins SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Build ${env.BUILD_NUMBER} completed successfully.\n\nCheck Jenkins: ${env.BUILD_URL}",
            to: "shivamkdas@gmail.com"
        )
    }

    failure {
        echo 'Pipeline failed. Please check the build logs.'
        emailext(
            subject: "Jenkins FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Build ${env.BUILD_NUMBER} failed.\n\nCheck Jenkins: ${env.BUILD_URL}",
            to: "shivamkdas@gmail.com"
        )
    }
}

}
