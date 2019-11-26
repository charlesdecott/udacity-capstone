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

                        sh 'hadolint Dockerfile'



                    }
        }
    }
}
