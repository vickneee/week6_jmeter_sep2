pipeline {
    agent any

    environment {
        JAVA_HOME = "/Library/Java/JavaVirtualMachines/amazon-corretto-17.jdk/Contents/Home"
        JMETER_HOME = "/opt/homebrew/opt/jmeter"
        PATH = "${JAVA_HOME}\\bin;${JMETER_HOME}\\bin;${env.PATH}"
    }

    tools {
        maven 'Maven3'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Non-Functional Test') {
            steps {
                sh 'jmeter -n -t tests/performance/demo.jmx -l result.jtl'
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
