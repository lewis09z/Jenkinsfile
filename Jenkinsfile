pipeline {
    agent any
    
    tools {
        // Gọi công cụ đã liên kết thành công với giao diện Jenkins Tools
        'hudson.plugins.sonar.SonarRunnerInstallation' 'sonar-scanner-tool' 
    }
    
    environment {
        // 1. Tên máy chủ SonarQube đã cấu hình trong Manage Jenkins > System
        SONAR_SERVER_NAME = 'SonarQube-Server'
        
        // 2. Ép chính xác đường dẫn thư mục bin (Trường hợp B) vào đầu biến PATH
        PATH = "/var/jenkins_home/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonar-scanner-tool/bin:${env.PATH}"
    }
    
    stages {
        stage('Checkout Source Code') {
            steps {
                // Tải mã nguồn từ kho lưu trữ GitHub hiện tại
                git branch: 'master', url: 'https://github.com/lewis09z/Jenkinsfile'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                // Sử dụng môi trường kết nối đến SonarQube Server
                withSonarQubeEnv("${SONAR_SERVER_NAME}") {
                    // Chạy lệnh quét mã nguồn
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
                // Đợi SonarQube xử lý phân tích và trả về kết quả
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
