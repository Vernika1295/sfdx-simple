#!groovy
import groovy.json.JsonSlurperClassic

def HUB_ORG=env.HUB_ORG
    def SFDC_HOST = env.SFDC_HOST
    def JWT_KEY_CRED_ID = env.JWT_KEY_CRED_ID
    def CONNECTED_APP_CONSUMER_KEY = env.CONNECTED_APP_CONSUMER_KEY
	def SFDC_USERNAME=env.SFDC_USERNAME
	def workspace
	
	
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
    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }
    
 //  stage('Example') {
	//	echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
	//}
		
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
		stage('Push To Test Org') {

            rc = sh returnStatus: true, script: "${toolbelt}/sfdx force:source:push --targetusername ${SFDC_USERNAME}"

            if (rc != 0) {

                error 'push failed'

            }

            // assign permset

            rc = sh returnStatus: true, script: "${toolbelt}/sfdx force:user:permset:assign --targetusername ${SFDC_USERNAME} --permsetname DreamHouse"

            if (rc != 0) {

                error 'permset:assign failed'

            }
        }

 

        stage('Run Apex Test') {

            sh "mkdir -p ${RUN_ARTIFACT_DIR}"

            timeout(time: 120, unit: 'SECONDS') {

                rc = sh returnStatus: true, script: "${toolbelt}/sfdx force:apex:test:run --testlevel RunLocalTests --outputdir ${RUN_ARTIFACT_DIR} --resultformat tap --targetusername ${SFDC_USERNAME}"

                if (rc != 0) {

                    error 'apex test run failed'

                }

            }

        }
 
        stage('collect results') {

            junit keepLongStdio: true, testResults: 'tests/**/*-junit.xml'

        }

    }
    
}
