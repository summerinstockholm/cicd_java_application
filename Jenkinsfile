pipeline{
    agent any
    agent{
        label "node"
    }
    stages{
        stage("Sonar quality check"){
            agent{
                docker {
                    image 'openjdk:11'
                }
            }
            steps{
                echo "Start sonar quality check"
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
                    }

                
                }
            }
        }
    }

}