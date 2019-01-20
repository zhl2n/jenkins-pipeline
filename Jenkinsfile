pipeline {
    agent any
    environment {
        CC = 'clang'
    }
    stages {
        stage('build') {
            steps {
            sh '/usr/local/bin/mvn --version'
            echo "${env.CC}"
            }
        }
//input message: "Does http://localhost:8888/staging/ look good?"
        stage('approve?') {
          //  input {
          //       message 'Does http://localhost:8888/staging/ look good?'
          //  } 
            steps {
                echo "waiting for approve"
            }
        }

        stage('deploy') {

            steps {
              lock("myresource") {
                  sh '/bin/hostname -f'
                  sh 'sleep 3'
             //     echo "deployed ${BUILD_NUMBER} from ${JENKINS_URL}"
                  echo "deployed ${env.BUILD_NUMBER} from ${env.JENKINS_URL}"
                  sh 'printenv'
                }
            }
        }
    }
}
