pipeline {
    agent any
    
    tools {
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
        
        stage('Thám Thính Đường Dẫn Tool') {
            steps {
                withSonarQubeEnv("${SONAR_SERVER_NAME}") {
                    // Lệnh này bắt Jenkins in ra danh sách thư mục con để xem tên chính xác là gì
                    sh '''
                        echo "=== BẮT ĐẦU KIỂM TRA THƯ MỤC THẬT ==="
                        ls -la /var/jenkins_home/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonar-scanner-tool/
                        echo "======================================"
                    '''
                }
            }
        }
    }
}
