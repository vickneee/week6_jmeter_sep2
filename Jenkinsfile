pipeline {
    agent any

    environment {
        JMETER_HOME = "/opt/homebrew/opt/jmeter"
        PATH = "${JMETER_HOME}\\bin;${env.PATH}"
    }

    tools {
        maven 'Maven3'
        jdk 'JAVA_17'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Non-Functional Test') {
            steps {
                sh '/opt/homebrew/opt/jmeter -n -t tests/performance/demo.jmx -l result.jtl'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'result.jtl', allowEmptyArchive: true
            perfReport sourceDataFiles: 'result.jtl'
        }
    }
}
