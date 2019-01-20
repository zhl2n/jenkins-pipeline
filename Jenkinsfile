pipeline {
    agent any
    environment {
        // Using returnStatus
        EXIT_STATUS = """${sh(
                returnStatus: true,
                script: 'exit 1'
            )}"""
        // Using returnStdout
        CC = """${sh(
                returnStdout: true,
                script: 'echo "clang"'
            ).trim()}""" 
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
            environment {
                DEBUG_FLAGS = '-g'
            }

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
