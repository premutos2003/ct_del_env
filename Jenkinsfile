def userInput = true
def didTimeout = false
node {
     stage("Delete Infrastructure") {
            deleteDir()
            sh '''
            echo ${PROJECT_NAME}
            export AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY}
            export AWS_SECRET_ACCESS_KEY=${AWS_SECRET_KEY}
            git clone https://github.com/premutos2003/ct-infra-base.git
            cd ./ct-infra-base/
            aws s3 cp s3://${ENV}-${REGION}-infra-base/state/terraform.tfstate ./terraform.tfstate  --region ${REGION}
            terraform init
            terraform destroy  -force -var env=${ENV}  -var region=${REGION} -force  -var aws_access_key=${AWS_ACCESS_KEY} -var aws_secret_key=${AWS_SECRET_KEY}
           '''
        }
        stage("Remove from DB") {

                    sh '''
                        curl -X POST -H 'content-type: application/json' -d '{"env":'${ENV}',"region":'${REGION}'}' host.docker.internal:3000/envs/delete
                   '''
                }

}