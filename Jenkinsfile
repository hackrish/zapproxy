pipeline {
    agent any

    stages {
        
        stage('ZAPSetup') {
            steps {
                script {
                    startZap(host: "127.0.0.1", port: 9091, timeout:500, zapHome: "C:/Krishna/ZedAttackProxy", sessionPath:"file:///C:/Program%20Files/OWASP/session.session", allowedHosts:['https://appsec-targ11.pd.local:8443']) // Start ZAP at /opt/zaproxy/zap.sh, allowing scans on github.com (if allowedHosts is not provided, any local addresses will be used
                }
            }
        }
        stage('Build & Test') {
            steps {
                script {
                    sh "mvn verify -Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=9091 -Dhttps.proxyHost=127.0.0.1 -Dhttps.proxyPort=9091" // Proxy tests through ZAP
                }
            }
        } 
        
    }
    post {
        always {
            script {
                archiveZap(failAllAlerts: 1, failHighAlerts: 0, failMediumAlerts: 0, failLowAlerts: 0, falsePositivesFilePath: "zapFalsePositives.json")
            }
        }
    }
}
