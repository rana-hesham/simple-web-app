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

                sh 'docker build --pull --rm -f "Dockerfile" -t botit:latest "."'
 
                }    
            }                                    
        }
    }
}
