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
    
    
}
