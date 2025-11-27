pipeline {
    agent any

    environment {
        JMETER_HOME = "/opt/homebrew/opt/jmeter"
    }

    tools {
        maven 'Maven3' // Maven tools in Jenkins
        jdk 'JAVA_17' // Java tools in Jenkins
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Non-Functional Test') {
            steps {
                sh '${env.JMETER_HOME}/bin/jmeter -n -t tests/performance/demo.jmx -l result.jtl'
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
