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
    }
}
