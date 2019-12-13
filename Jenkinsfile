pipeline{
    agent{
        label "jenkins"
    }
    stages{
        stage("Docker login"){
            steps{
                echo "========executing docker login========"
                withCredentials([usernamePassword(credentialsId: 'dockerhub_san360', usernameVariable:'USERNAME',passwordVariable:'PASSWORD')]){
                    sh(
                        label: "login docker hub",
                        script: "docker login --username  $USERNAME --password $PASSWORD"
                    )
                }
            }
        }
        stage("Docker build"){
            steps{
                echo "========executing A========"
                sh(
                    label: "build docker",
                    script: "docker build --no-cache -t san360/static:$BUILD_NUMBER ."
                )
            }
        }
        stage("Docker push"){
            steps{
                echo "========executing Docker push========"
                sh(
                    label: "push docker images",
                    script: "docker push san360/static:$BUILD_NUMBER"
                )
            }
        }
        stage("Docker deploy"){
            steps{
                echo "========executing Docker push========"
                 withCredentials([usernamePassword(credentialsId: 'i360_login', usernameVariable:'USERNAME',passwordVariable:'PASSWORD')]){

                    script {
                        sh "sshpass -p '$PASSWORD' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker pull san360/static:${env.BUILD_NUMBER}\""
                        try {
                            sh "sshpass -p '$PASSWORD' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker stop train-schedule\""
                            sh "sshpass -p '$PASSWORD' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker rm train-schedule\""
                        } catch (err) {
                            echo: 'caught error: $err'
                        }
                        sh "sshpass -p '$PASSWORD' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker run --restart never --name train-schedule -p 8080:8080 -d san360/static:${env.BUILD_NUMBER}\""
                    }
                }
            }
        }
    }
}
