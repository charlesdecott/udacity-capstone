pipeline {
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
        },
            stage('Build docker image') {
                    steps {

                        sh 'docker build --tag=nannabat/publicyard:testUdacitycapstone .'



                    }
        }
    }
}
