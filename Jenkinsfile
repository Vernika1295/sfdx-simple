#!groovy
import groovy.json.JsonSlurperClassic
 pipeline {
    agent any
    environment {
        // Removed other variables for clarity...
        SFDX_USE_GENERIC_UNIX_KEYCHAIN = true
        // ...
    }
node {
    def BUILD_NUMBER = env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR = "tests/${BUILD_NUMBER}"
    def SFDC_USERNAME
    def HUB_ORG = env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY = env.CONNECTED_APP_CONSUMER_KEY_DH
    def toolbelt = tool 'toolbelt'

  
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
}
