def branch = "main"
def remote = "origin"
def directory = "~/wayshub-frontend"
def server = "fama@103.191.92.211"
def cred = "wayshub1"
def image = "nobody1305/fama-frontend:latest"

pipeline{
    agent any
    stages{
        stage('repo pull'){
            steps{
                sshagent([cred]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
		    docker compose down
                    cd ${directory}
                    git pull ${remote} ${branch}
                    exit
                    EOF"""
                    }
                }
            }

	 stage('docker build'){
            steps{
                sshagent([cred]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker build -t fama-frontend .
                    exit
                    EOF"""
                    }
                }
            }

        stage('docker compose'){
            steps{
                sshagent([cred]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker compose up -d
                    exit
                    EOF"""
                    }
                }
            }
	
	 stage('docker push'){
            steps{
                sshagent([cred]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
		    docker tag fama-frontend:latest ${image}
                    docker push ${image}
                    exit
                    EOF"""
                    }
                }
            }
        }
    }

discordSend description: "Jenkins Pipeline Build", footer: "build succes", link: env.BUILD_URL, result: currentBuild.currentResult, title: wayshub-frontend, webhookURL: "https://discordapp.com/api/webhooks/1129367074859397140/B2AL8n5a9Teg2FKSa82tuemNOu3SBe4XenTefkM6Q59vXpniLObl-ev0CIa9DdSWAGcQ"
