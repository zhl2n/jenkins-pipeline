pipeline {
    agent any
    options {
        //preserveStashes() 
        preserveStashes(buildCount: 5) 
    }
    parameters {
        string(name: 'Greeting', defaultValue: 'Hello', description: 'How should I greet the world?')
    }
    environment {
	// credentials were added first using secret text in Jenkins
        AWS_ACCESS_KEY_ID     = credentials('aws_access_key_id')
        AWS_SECRET_ACCESS_KEY = credentials('aws_secret_access_key')

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
        stage('parallel') {
            parallel {
                stage('Branch A') {
                    steps {
                        echo "On Branch A"
			sleep 3
                    }
                }
                stage('Branch B') {
                    steps {
                        echo "On Branch B"
			sleep 10
                    }
                }
 	    }
	}
        stage('build') {
            steps {
            sh '/usr/local/bin/mvn --version'
            echo "${env.CC}"
	    echo "${params.Greeting} World!"
	    echo "${BRANCH_NAME}"
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

    post {
        always {
//            junit '**/target/*.xml'
	      echo "post action"
        }
        failure {
//            mail to: 'brewster.zhang@gmail.com', subject: 'The Pipeline failed :( '
 	    mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "brewster.zhang@gmail.com"; 
        }
    }
}
