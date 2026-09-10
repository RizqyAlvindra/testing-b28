def secret = 'mentor'
def server = 'mentor@188.166.229.119'
def directory = 'testing-b28'
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
                    docker compose build
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
                    docker compose down
		    docker compose up -d
                    exit
                    EOF"""
                }
            }
        }
    }
}
