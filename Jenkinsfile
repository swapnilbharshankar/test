
pipeline {
    agent any
    triggers { 
		pollSCM('H */4 * * 1-5') 
	}
    environment {
        index = "index.html"
    } // end of environment
    stages {
        stage('GENERATE-REPORTING-BASE-STRUCTURE') {
            steps {
                //create index.html
                createIndexTemp(
                    "${index}",
                    "${JOB_NAME}",
                    "${BUILD_NUMBER}",
                    "${BUILD_URL}",
                    "${JOB_URL}"
                )
            }
        } // end of GENERATE-REPORTING-BASE-STRUCTURE
        stage('ENV PREPARATION') {
            steps {
                script {
                    try {
                        sh "echo aaa"
                    }
                    catch (exc) {
                        echo 'Failed to Checkout SCM'
                        throw exc
                    }
                }
            }
            post
            {
                success
                {
			echo "PASS"
                }
                failure
                {
			echo "FAIL"
                }
            }        
        } // end of ENV PREPARATION
    } // End of stages
} // End of pipeline

