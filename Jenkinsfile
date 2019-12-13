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
                    script: "docker build --no-cache -t san360/static:$BUILD_NUMBER"
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
    }
}
