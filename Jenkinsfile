def builderDocker
def CommitHash

pipeline {
    agent any 

    parameters {
        booleanParam(name: 'Run', defaultValue: true, description: 'Toggle this value for testing')
        choice(name: 'CICD', choices: ['CICD', 'CI'], description: 'pick CI / CD, CD, or Rollback')
        
    }
    stages {
        stage('Clone Project') {
            steps{
               script {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'deploy',
                                verbose: false,
                                transfers: [
                                    sshTransfer(
                                        execCommand: 'cd go-app; git pull',
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }

        stage('Build Project') {
            steps{
               script {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'deploy',
                                verbose: false,
                                transfers: [
                                    sshTransfer(
                                        execCommand: 'cd go-app; docker build -t endaafiandika/go-app:1 .',
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
        
        stage('push Image') {
            when {
                expression {
                    CICD == 'CICD'
                }
            }
            steps{
               script {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'deploy',
                                verbose: false,
                                transfers: [
                                    sshTransfer(
                                        execCommand: 'docker push endaafiandika/go-app:1; docker image prune -f',
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
        stage('Deployment') {
            when {
                expression {
                    CICD == 'CICD'
                }
            }
            steps{
               script {
                   sh "sudo kubectl apply -f deploy-k8s.yaml"  
                }
            }
        }
        stage('Run Testing Development') {
            when {
                expression {
                    CICD == 'CICD'
                }
            }
            steps{
                script {
                    sh 'echo passed'
                }
            }
        }
    }
}