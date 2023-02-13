pipeline {
    agent any
    stages {
        boolean testPassed = true
        stage(test) {
            try{
                sh 'pip install flask'
                sh 'python3 test_hello.py'
            }catch (Exception e){
                testPassed = false
            }
        }
        stage(build) {
            if(testPassed){
                steps {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'username', passwordVariable: 'pass')]) {
                        sh 'docker login -u ${username} -p ${pass}'
                        sh 'docker build --pull --rm -f "Dockerfile" -t botit:latest "."'
                        sh 'docker tag botit:latest ranahesham/botit:v1.1'
                        sh 'docker image push ranahesham/botit:v1.1'
                    }
                }
            }else{
                emailext body: 'Test failed: Check console output at $BUILD_URL to view the results',
                to: "rana.hesham2017@gmail.com", 
                subject: 'Test failed in Jenkins: $PROJECT_NAME - #$BUILD_NUMBER' 
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
