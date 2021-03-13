@Library('piper-lib-os') _
@Library('ares-lib') __

def development = [ 
	"hostname": "<dev IP>", // <------------- past development ABAP system IP here
	"port": "8000",
	"client": "100",
	"credentials": "gCTS_source",
	"repo_id": "<int repo name>" // <------------- past your gCST repository name here
    ]

def integrate = [ 
	"hostname": "<int IP>", // <------------- past integration ABAP system IP here
	"port": "8000",
	"client": "500",
	"credentials": "gCTS_target",
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
                    development.pipelinename = "${env.JOB_BASE_NAME}_${env.BUILD_NUMBER}"
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
                        abapCredentialsId: 'gCTS_source',
                        repository: '<int repo name>' // <------------- past your gCST repository name here
                        )
                } //script
            } //steps
        } //stage        
    } //stages
}//pipeline
