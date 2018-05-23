#!groovy

import groovy.json.JsonSlurperClassic

def workspace

def BUILD_NUMBER=env.BUILD_NUMBER

    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"

    def SFDC_USERNAME

 

    def HUB_ORG=env.HUB_ORG

    def SFDC_HOST = env.SFDC_HOST_DH

    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH

    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY

 

    def toolbelt = tool 'toolbelt'


node
{
        stage('checkout') {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '32062537', url: 'https://github.com/Vernika1295/sfdx-simple']]])
         workspace = pwd()
        }
        stage('Static Code Analysis') {
        
                echo 'Testing..'
            }
        
        stage('Deploy') {
    
                echo 'Deploying....'
            }
        
        stage('Build') {
        
                echo 'Building..'
            }
     withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {

        stage('Create Scratch Org') {

 

            rc = sh returnStatus: true, script: "${toolbelt}/sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"

            if (rc != 0) { error 'hub org authorization failed' }

 

            // need to pull out assigned username

            rmsg = sh returnStdout: true, script: "${toolbelt}/sfdx force:org:create --definitionfile config/project-scratch-def.json --json --setdefaultusername"

            printf rmsg

            def jsonSlurper = new JsonSlurperClassic()

            def robj = jsonSlurper.parseText(rmsg)

            if (robj.status != "ok") { error 'org creation failed: ' + robj.message }

            SFDC_USERNAME=robj.username

            robj = null

 

        }

     }
}
