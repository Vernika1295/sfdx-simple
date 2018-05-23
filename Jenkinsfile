#!groovy
import groovy.json.JsonSlurperClassic
node {
    def BUILD_NUMBER = env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR = "tests/${BUILD_NUMBER}"
    def SFDC_USERNAME
    def HUB_ORG = env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY = env.CONNECTED_APP_CONSUMER_KEY_DH
    def toolbelt = tool 'toolbelt'

    stage('Retieve Source') {
        checkout scm
    }

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Authorise Dev Hub') {
            rc = sh returnStatus: true, script: "${toolbelt}/sfdx force:auth:jwt:grant -i ${CONNECTED_APP_CONSUMER_KEY} -u ${HUB_ORG} -f ${jwt_key_file} -d"
            if(rc != 0) {
                error 'Hub Org authorisation failed'
            }
        }
    }
}
