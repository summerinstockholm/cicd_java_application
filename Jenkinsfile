pipeline{
    agent any
    agent{
        label "node"
    }
    stages{
        stage("Sonar quality check"){
            agent{
                docker{
                    image 
                }
            }
            steps{
                echo "Start sonar quality check"
                script{

                }
            }
        }
    }
    post{
        always{
            echo "SUCCESS"
        }
    }
}