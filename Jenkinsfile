pipeline {
    agent any
    stages {
        stage(test) {
            steps {
                sh 'pip install flask'
                sh 'python3 test_hello.py'
            }                                    
        }
        stage(build) {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'username', passwordVariable: 'pass')]) {
                    sh 'docker login -u ${username} -p ${pass}'
                    sh 'docker build --pull --rm -f "Dockerfile" -t botit:latest "."'
                    sh 'docker tag botit:latest ranahesham/botit:v1.1'
                    sh 'docker image push ranahesham/botit:v1.1'
                }
            }
        }
    }
    post {
        always {
          script {
            if (currentBuild.currentResult != 'FAILURE') {
              step([$class: 'Mailer', notifyEveryUnstableBuild: true, recipients: "rana.hesham2017@gmail.com", sendToIndividuals: true])
            }
          }
        }
   }
}
