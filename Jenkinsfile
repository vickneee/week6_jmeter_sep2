pipeline {
    agent any

    environment {
        PATH = "/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:${env.PATH}"
        JMETER_HOME = "/opt/homebrew/opt/jmeter" // JMeter installation path
    }

    tools {
        maven 'Maven3' // Maven tool configured in Jenkins
        jdk 'JAVA_17' // JDK configured in Jenkins
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Non-Functional Test') {
            steps {
                sh "${env.JMETER_HOME}/bin/jmeter -n -t tests/performance/demo.jmx -l result.jtl"
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
