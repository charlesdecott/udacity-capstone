pipeline {
    environment {
    registry = "nannabat/publicyard"
    registryCredential = 'dockerhub'
}
    agent any

    stages {
            stage('Lint HTML') {
                steps {

                    sh 'tidy -q -e *.html'



                }
        }
            stage('Lint Dockerfile') {
                    steps {

                        sh 'hadolint --ignore DL3006 --ignore SC2028 --ignore DL3008 --ignore DL3015 Dockerfile'



                    }
        }
            stage('Build docker image') {
                    steps {

                        withCredentials([usernamePassword( credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh 'echo "the value of username is ${USERNAME} and password id ${PASSWORD}"'
                        sh "docker login -u ${USERNAME} -p ${PASSWORD}"
                        sh 'docker build --tag=nannabat/publicyard:udacityTestCapstone .'



                    }
        }
        }
            stage('Push docker image') {
                    steps {

                        sh 'docker push nannabat/publicyard:udacityTestCapstone'



                    }
        }
            stage('set kubectl context to backup') {
                    steps {

                        sh "kubectl --kubeconfig='/var/lib/jenkins/config'  config use-context arn:aws:eks:us-west-2:595702470973:cluster/backup"



                    }
        }
            stage('create backup replication controller') {
                    steps {

                        sh "kubectl apply -f ./backup-controller.json"



                    }
        }
    }
}
