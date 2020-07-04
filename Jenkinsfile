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
            stage('Deploy latest on backup cluster') {
                    steps {

                        sh "kubectl config use-context arn:aws:eks:us-west-2:595702470973:cluster/backup"
                        sh "kubectl apply -f ./backup-controller.json"



                    }
        }
            stage('Approval to route traffic to backup') {
                    steps {
                        input "Does the new version looks good?"
                    }
        }
             stage('Routing traffic to backup') {
                    steps {
                        sh "kubectl apply -f ./backup-service.json"
                        sh '''ELB="$(kubectl get svc blue -o=jsonpath='{.status.loadBalancer.ingress[0].hostname}')"
                              aws route53 change-resource-record-sets --hosted-zone-id ZDQ7GFDSQJGM6 --change-batch '{ "Comment": "creating a record set",  "Changes": [ { "Action": "UPSERT", "ResourceRecordSet": { "Name": "capstone.getsabze.com", "Type": "CNAME", "TTL": 120, "ResourceRecords": [ { "Value": "'"$ELB"'" } ] } } ] }'
                            '''
                        
                    }
        }
            stage('Deploy latest on production cluster') {
                    steps {
                        sh "kubectl config use-context arn:aws:eks:us-west-2:595702470973:cluster/prod"
                        sh "kubectl apply -f ./prod-controller.json"



                    }
        }
            stage('Approval to route traffic to prod') {
                    steps {
                        input "Does the new version on prod looks good?"
                    }
        }
            stage('Routing traffic to prod') {
                    steps {
                        sh "kubectl apply -f ./prod-service.json"
                        sh '''ELB="$(kubectl get svc green -o=jsonpath='{.status.loadBalancer.ingress[0].hostname}')"
                              aws route53 change-resource-record-sets --hosted-zone-id ZDQ7GFDSQJGM6 --change-batch '{ "Comment": "creating a record set",  "Changes": [ { "Action": "UPSERT", "ResourceRecordSet": { "Name": "capstone.getsabze.com", "Type": "CNAME", "TTL": 120, "ResourceRecords": [ { "Value": "'"$ELB"'" } ] } } ] }'
                            '''
                        
                    }
        }

    }
}
