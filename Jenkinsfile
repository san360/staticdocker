pipeline{
    agent{
        label "jenkins"
    }
    stages{
        stage("Docker build"){
            steps{
                echo "========executing A========"
                sh(
                    label: "build docker",
                    script: "docker build --no-cache -t san360/static:$IMAGE_VERSION"
                )
            }
        }
    }
}
