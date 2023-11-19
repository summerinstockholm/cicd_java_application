pipeline{
    agent any
    parameters {
        booleanParam(name: 'skip_sonar', defaultValue: false, description: "Установите в true для того чтобы пропустить проверки сонара (они сейчас не работают)")
    }
    environment {
        NEXUS_USER = credentials('nexus-user')
        NEXUS_PASSWORD = credentials('nexus-password')
        NEXUS_REPOSITORY_ADDRESS = '158.160.84.218:8083'
        VERSION = '${env.BUILD_NUMBER}'
    }
    stages{
        stage("Sonar quality check"){
            when { 
                expression {
                    params.skip_sonar != true }
                }
            agent {
                docker {
                    image 'openjdk:11'
                    args '-v /var/run/docker.sock:/var/run/docker.sock  -u root:root'
                }
            }
            steps{
                echo "Start sonar quality check"
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube --stacktrace --warning-mode all'
                    }
                    timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }               
                }
            }
        }
        stage ("Docker build and docker push"){
            steps{
                script{
                    sh '''
                    docker build -t ${NEXUS_REPOSITORY_ADDRESS}/springapp:${VERSION} .
                    echo ${NEXUS_PASSWORD} | docker login ${NEXUS_REPOSITORY_ADDRESS} -u ${NEXUS_USER} --password-stdin
                    docker push ${NEXUS_REPOSITORY_ADDRESS}/springapp:${VERSION}
                    docker rmi ${NEXUS_REPOSITORY_ADDRESS}/springapp:${VERSION}
                    docker logout
                    '''
                }
            }
        }
    }

}