pipeline {
    agent any

    environment {
        JAVA_17 = "/Users/victoriavavulina/Library/Java/JavaVirtualMachines/corretto-17.0.14/Contents/Home"
        JMETER_HOME = "/opt/homebrew/opt/jmeter"
        PATH = "${JAVA_17}\\bin;${JMETER_HOME}\\bin;${env.PATH}"
    }

    tools {
        maven 'Maven3'
        java
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
