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

                script {
                    def files = [
                        'index.html',
                        'style.css',
                        'script.js'
                    ]

                    def zipFile = new File('railway-reservation.zip')

                    if (zipFile.exists()) {
                        zipFile.delete()
                    }

                    def zipOutputStream =
                        new java.util.zip.ZipOutputStream(
                            new java.io.FileOutputStream(zipFile)
                        )

                    try {
                        files.each { fileName ->
                            def file = new File(fileName)

                            if (!file.exists()) {
                                error("ERROR: ${fileName} not found")
                            }

                            def entry =
                                new java.util.zip.ZipEntry(fileName)

                            zipOutputStream.putNextEntry(entry)

                            file.withInputStream { input ->
                                byte[] buffer = new byte[4096]
                                int bytesRead

                                while ((bytesRead = input.read(buffer)) != -1) {
                                    zipOutputStream.write(buffer, 0, bytesRead)
                                }
                            }

                            zipOutputStream.closeEntry()
                        }
                    } finally {
                        zipOutputStream.close()
                    }

                    echo 'railway-reservation.zip created successfully'
                }
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