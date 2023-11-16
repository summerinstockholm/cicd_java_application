pipeline{
    agent any
    stages{
        stage("Sonar quality check"){
            agent {
                docker {
                    image 'openjdk:8'
                }
            }
            steps{
                echo "Start sonar quality check"
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube Dsonar.userHome=`pwd`/.sonar --stacktrace --warning-mode all'
                    }                
                }
            }
        }
    }

}