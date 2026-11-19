pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building Railway Reservation application'

                sh '''
                    echo "Validating application files..."

                    test -f index.html
                    test -f style.css
                    test -f script.js

                    echo "Build validation successful!"
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Testing application files'

                sh '''
                    echo "Running basic application tests..."

                    test -s index.html
                    test -s style.css
                    test -s script.js

                    echo "All tests passed!"
                '''
            }
        }

        stage('Package') {
            steps {
                echo 'Creating application package'

                sh '''
                    rm -f railway-reservation.zip
                    zip railway-reservation.zip index.html style.css script.js
                '''
            }
        }
    }

    post {
        success {
            echo 'Railway Reservation Build Successful!'
        }

        failure {
            echo 'Railway Reservation Build Failed!'
        }
    }
}