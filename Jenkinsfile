pipeline {

    agent any
    
    stages {

        stage('Build') {
            steps {
                echo "We are in build phase"
            }

            post {
                success {
                   echo "We are in build phase post success"
                }
            }
        }

        stage('Test') {
            steps {
                sh '''
                  echo "We are in the test  phase"
                  /usr/bin/mvn test -f bibin-war-src
                '''
            }

            post {
                always {
                    echo "We are in always post test phase"
                }
            }
        }

        stage('Push') {
            steps {
                echo "We are in build push phase"
            }
        }

        stage('Deploy') {
            steps {
                echo "We are in deploy phase"
            }
        }
    }
}
