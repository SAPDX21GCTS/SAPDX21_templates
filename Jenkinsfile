@Library('piper-lib-os') _
@Library('ares-lib') __

def integrate = [ 
	"hostname": "<int IP>", // <------------- past integration ABAP system IP here
	"port": "8000",
	"client": "500",
	"credentials": "<int cred ID>", // <------------- past your Jenkins credentials ID for inr here
	"repo_id": "<int repo name>" // <------------- past your gCST repository name here
]

pipeline {
    agent any
    stages {
        stage('Setup') {
            steps {
                git url: '<github repo URL>' // <------------- past your GitHub repository here
                setupCommonPipelineEnvironment script: this
            } //steps
        } //stage
        stage('Integrate') {
            steps {
                script {
                    integrate.pipelinename = "${env.JOB_BASE_NAME}_${env.BUILD_NUMBER}"
                    integrate.commit_id = sh(returnStdout: true, script: 'git rev-parse HEAD')
                    echo 'integrate.commit_id is: '+integrate.commit_id
                    integrate.transport = pullByCommitgCTSHTTP(integrate)
                }
            } //steps
        } //stage        
        stage ('AUnit') {
            steps {
                script {
                    gctsExecuteABAPUnitTests(
                        script: this,
                        host: 'http://<int IP>:8000', // <------------- past integration ABAP system IP here
                        client: '500',
                        abapCredentialsId: '<int cred ID>', // <------------- past your Jenkins credentials ID for inr here
                        repository: '<int repo name>' // <------------- past your gCST repository name here
                        )
                } //script
            } //steps
        } //stage        
    } //stages
}//pipeline
