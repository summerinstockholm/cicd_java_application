pipeline{
    agent any
    parameters {
        booleanParam(name: 'skip_sonar', defaultValue: false, description: "Установите в true для того чтобы пропустить проверки сонара (они сейчас не работают)")
    }
    stages{
        stage("Sonar quality check"){
            when { 
                expression {
                    params.skip_sonar != true }
                }
                echo "Sonar quality check were skipped"
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
            steps{
                echo "Start sonar quality check"
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube --stacktrace --warning-mode all'
                    }                
                }
            }
        }
    }

}