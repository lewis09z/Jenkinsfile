pipeline {
    agent any
    
    tools {
        // Điền chính xác Class định danh thu được từ log lỗi
        'hudson.plugins.sonar.SonarRunnerInstallation' 'sonar-scanner-tool' 
    }
    
    environment {
        SONAR_SERVER_NAME = 'SonarQube-Server'
    }
    
    stages {
        stage('Checkout Source Code') {
            steps {
                git branch: 'master', url: 'https://github.com/lewis09z/Jenkinsfile'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONAR_SERVER_NAME}") {
                    sh '''
                        sonar-scanner \
                        -Dsonar.projectKey=Sonar.project.full \
                        -Dsonar.projectName="Sonar.project.full" \
                        -Dsonar.sources=. \
                        -Dsonar.sourceEncoding=UTF-8
                    '''
                }
            }
        }
        
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
