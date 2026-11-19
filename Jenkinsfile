pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Downloading Railway Reservation source code'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Starting application build'

                sh '''
                    if [ ! -f index.html ]; then
                        echo "ERROR: index.html not found"
                        exit 1
                    fi

                    if [ ! -f style.css ]; then
                        echo "ERROR: style.css not found"
                        exit 1
                    fi

                    if [ ! -f script.js ]; then
                        echo "ERROR: script.js not found"
                        exit 1
                    fi

                    echo "Application files verified successfully"
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Testing application files'

                sh '''
                    grep -qi "<html" index.html
                    if [ $? -ne 0 ]; then
                        echo "ERROR: HTML tag not found"
                        exit 1
                    fi

                    grep -qi "bookingForm" index.html
                    if [ $? -ne 0 ]; then
                        echo "ERROR: bookingForm not found"
                        exit 1
                    fi

                    grep -qi "addEventListener" script.js
                    if [ $? -ne 0 ]; then
                        echo "ERROR: addEventListener not found"
                        exit 1
                    fi

                    echo "All basic validation tests passed"
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