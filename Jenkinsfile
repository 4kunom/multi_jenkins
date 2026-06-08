// 프로젝트 최상위에 'Jenkinsfile' 이라는 이름으로 저장
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out branch: ${env.BRANCH_NAME}"
            }
        }
        
        stage('Test') {
            steps {
                echo "Running tests for ${env.BRANCH_NAME}..."
                // 예: sh 'npm run test' 또는 sh './gradlew test'
            }
        }

        stage('Deploy to Dev') {
            // develop 브랜치일 때만 개발 서버 배포 실행
            when {
                branch 'develop'
            }
            steps {
                echo "Deploying to Development Server..."
                // 개발 배포 스크립트 작성
            }
        }

        stage('Deploy to Production') {
            // main(또는 master) 브랜치일 때만 운영 서버 배포 실행
            when {
                branch 'main'
            }
            steps {
                echo "Deploying to Production Server..."
                // 운영 배포 스크립트 작성
            }
        }
    }
    
    post {
        always {
            echo "Pipeline finished."
        }
    }
}
