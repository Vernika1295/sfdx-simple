pipeline {
    agent any
    environment {
        // Removed other variables for clarity...
        SFDX_USE_GENERIC_UNIX_KEYCHAIN = true
        // ...
    }
    stages {    
        stage('TEST') {
            steps {
                withCredentials([file(credentialsId: 'jenkins-cert', variable: 'VAR_CERT_FILE')]) {
                    sh returnStdout: true, script: "${SFDX_HOME}/sfdx force:auth:jwt:grant --clientid ${HUB_CLIENT_ID} --username ${HUB_USERNAME} --jwtkeyfile ${VAR_CERT_FILE} --setdefaultdevhubusername --instanceurl ${HUB_HOST}"
                }
            }
        }
    }
}
