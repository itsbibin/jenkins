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



        stage('Build Docker Image') {
            steps {
                sh '''
                echo "Building docker image"
                cp bibin-war-src/target/hello-2.0.war docker-build/
                cd docker-build/
                docker build -t myjenkinsapp .
                docker tag myjenkinsapp bkur/myappwar80:$BUILD_NUMBER
                '''
            }
        }


        stage('Push') {
            steps {
                sh '''
                echo "We are in build push phase"
                docker login -u bkur -p $REGPASS
                docker push bkur/myappwar80:$BUILD_NUMBER
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                echo "Deploying the new image to Kubernetes cluster"
                envsubst < k8/bibinapp_deploy.yaml | /usr/local/bin/kubectl apply -f -
                '''
            }
        }
    }
}
