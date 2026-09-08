def secret = 'mentor'
def server = 'mentor@146.190.101.236'
def directory = 'wayshub-backend'
def branch = 'main'

pipeline{
    agent any
    stages{
        stage ('pulling new code'){
         steps{
             sshagent([secret]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
		    git pull origin ${branch}
                    exit
                    EOF"""
                }
            }
        }
        stage ('build apps'){
            steps{
                sshagent([secret]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    echo "docker build"
                    exit
                    EOF"""
                }
            }
        }
        stage ('push to registry'){
            steps{
                sshagent([secret]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    echo "docker push"
                    exit
                    EOF"""
                }
            }
        }
        stage ('deploy'){
            steps{
                sshagent([secret]) {
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    echo "docker compose up -d"
                    exit
                    EOF"""
                }
            }
        }
    }
}
