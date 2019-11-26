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

                        sh 'hadolint disable=DL3006,SC2028,DL3008,DL3015 Dockerfile'



                    }
        }
    }
}
