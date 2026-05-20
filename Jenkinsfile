pipeline {
    agent any
    
    tools {
        // Khai báo công cụ liên kết với cấu hình trong Manage Jenkins > Tools
        'hudson.plugins.sonar.SonarRunnerInstallation' 'sonar-scanner-tool' 
    }
    
    environment {
        // 1. Tên máy chủ SonarQube đã cấu hình trong Manage Jenkins > System
        SONAR_SERVER_NAME = 'SonarQube-Server'
        
        // 2. Ép đường dẫn thư mục bin của Sonar Scanner vào biến môi trường PATH hệ thống
        PATH = "/var/jenkins_home/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonar-scanner-tool/sonar-scanner-8.1.0.6389-linux-x64/bin:${env.PATH}"
    }
    
    stages {
        stage('Checkout Source Code') {
            steps {
                // Tải mã nguồn từ kho lưu trữ GitHub của bạn
                git branch: 'master', url: 'https://github.com/lewis09z/Jenkinsfile'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                // Sử dụng môi trường kết nối đến SonarQube Server
                withSonarQubeEnv("${SONAR_SERVER_NAME}") {
                    // Thực thi lệnh quét mã nguồn
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
                // Đợi SonarQube xử lý phân tích và trả về kết quả (Pass/Fail)
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
