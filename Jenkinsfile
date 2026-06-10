pipeline {
    agent any
    
    environment {
        // ⚠️ 본인의 Docker Hub 계정명으로 변경하세요!
        DOCKERHUB_USER = 'aprkunni'
        IMAGE_NAME     = 'my-app'
        IMAGE_TAG      = "${env.BUILD_NUMBER}" // 빌드 번호를 태그로 사용 (예: my-app:1)
    }

    stages {
        stage('Checkout') {
            steps {
                // GitHub에서 코드를 가져옵니다.
                checkout scm
            }
        }

        stage('Docker Image Build') {
            steps {
                echo "🔨 도커 이미지 빌드 시작..."
                script {
                    // Dockerfile을 기반으로 이미지 생성 (예: your_id/my-app:1)
                    sh "docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} ."
                    // 최신 이미지를 가리키는 latest 태그도 함께 생성
                    sh "docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Docker Image Push') {
            steps {
                echo "🚀 Docker Hub로 이미지 업로드 중..."
                // 젠킨스에 등록한 Credentials ID를 사용해 안전하게 로그인
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        // Docker Hub 로그인 및 푸시
                        sh "echo '${DOCKER_PASS}' | docker login -u '${DOCKER_USER}' --password-stdin"
                        sh "docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
                        sh "docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:latest"
                    }
                }
            }
        }
        
        stage('Cleaning up') {
            steps {
                echo "🧹 호스트 서버 용량 관리를 위해 로컬 이미지 삭제..."
                sh "docker rmi ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
                sh "docker rmi ${DOCKERHUB_USER}/${IMAGE_NAME}:latest"
            }
        }
    }
}