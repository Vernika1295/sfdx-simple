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
                withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
                    sh returnStdout: true, script: "${SFDX_HOME}/sfdx force:auth:jwt:grant --clientid ${HUB_CLIENT_ID} --username ${vernika@irketa.com} --jwtkeyfile ${VAR_CERT_FILE} --setdefaultdevhubusername --instanceurl ${HUB_HOST}"
                }
            }
        }
    }
}

