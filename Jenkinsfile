pipeline {

    agent any
    
    
    environment {
        REGPASS = credentials('docker-hub-pass') 
    }

    stages {

        stage('Build') {
            steps {
                sh '''
                   echo "We are in build phase"
                   /usr/bin/mvn package -f bibin-war-src
                '''
            }

            post {
                success {
                   archiveArtifacts artifacts: 'bibin-war-src/target/*.war', fingerprint: true
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
                sh '''
                echo "We are in build push phase"
                docker login -u bkur -p $REGPASS
                docker tag bibin-war-src/target/*.war bkur/myappwar80:2.0
                docker push bkur/myappwar80:2.0
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo "We are in deploy phase"
            }
        }
    }
}
