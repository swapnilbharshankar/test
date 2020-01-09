pipeline {
    agent any
    triggers { pollSCM('* * * * *') }
    environment {
        index = "index.html"
    } // end of environment
    stages {
        stage('GENERATE-REPORTING-BASE-STRUCTURE') {
            steps {
		script {
			echo "a"
		}
            }
        } // end of GENERATE-REPORTING-BASE-STRUCTURE
        stage('ENV PREPARATION') {
            steps {
                script {
                    try {
                        dir('test') {
                            checkout([
                                $class: 'GitSCM',
                                branches: [[name: "*/master"]],
                                poll: false,
                                doGenerateSubmoduleConfigurations: false,
                                extensions: [],
                                submoduleCfg: [],
                                userRemoteConfigs: [[
                                    credentialsId: 'github',
                                    url: "https://github.com/swapnilbharshankar/test.git"
                                ]]
                            ])
                        }                        
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
                    stage_status_msg("${STAGE_NAME}","${currentBuild.currentResult}")
                }
                failure
                {
                    stage_status_msg("${STAGE_NAME}","${currentBuild.currentResult}")
                }
            }        
        } // end of ENV PREPARATION
    } // End of stages
} // End of pipeline

// function for email template

def createIndexTemp(index, JOB_NAME, BUILD_NUMBER, BUILD_URL, JOB_URL) {
  sh '''
    #!/bin/bash
    cat > index.html <<-EOF
    <html><head><title>$JOB_NAME Report</title><style>
    #TestCategory {
        font-family: "Trebuchet MS", Arial, Helvetica, sans-serif;
        width: 100%;
        border-collapse: collapse;
    }
    #TestCategory td, #TestCategory th {
        font-size: 1em;
        border: 1px solid #000000;
        padding: 3px 7px 2px 7px;
    }
    #TestCategory th {
        font-size: 1.1em;
        text-align: left;
        padding-top: 5px;
        padding-bottom: 4px;
        background-color: #1E90FF;
    }
    </style></head>
    <body>
    <font face="Trebuchet MS, Arial, Helvetica, sans-serif">
    <p><b>Build Number: </b>${BUILD_NUMBER}</p>
    <p><b>Logs: </b>
    <a href="${BUILD_URL}/Logs">All Logs</a></p>
    <p><b>Workflow View: </b>
    <a href="${JOB_URL}/workflow-stage/">Workflow</a></p>
    <table border="1" id="TestCategory">
    <tr><th>Step</th><th>Status</th></tr>
EOF
  '''
}


def stage_status_msg(stage_name, result) {
    script {
        echo "${stage_name} => ${result}"
        sh "echo '<tr><td>${stage_name}</td><td>' >> ${index}"
        if ( "${result}" == "SUCCESS" ) {
            sh "echo '<font color='green'>${result}</font></td></tr>' >> ${index}"
        } else {
            sh "echo '<font color='red'>${result}</font></td></tr>' >> ${index}"
        }
    }
}

