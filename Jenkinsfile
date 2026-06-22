pipeline {
    agent any
    
    environment {
        // 본인의 Docker Hub 계정명
        DOCKERHUB_USER = 'aprkunni' 
        IMAGE_NAME     = 'my-app'
        IMAGE_TAG      = "${env.BUILD_NUMBER}" // 빌드 번호를 태그로 사용 (예: 1, 2, 3...)
    }

    stages {
        stage('Checkout') {
            steps {
                echo "🌿 현재 빌드 중인 브랜치: ${env.BRANCH_NAME}"
                // 💡 젠킨스에게 깃허브 코드를 명확하게 워크스페이스로 긁어오라고 명령합니다.
                checkout scm
            }
        }

        stage('Docker Image Build') {
            steps {
                echo "🔨 도커 이미지 빌드 시작..."
                script {
                    // Dockerfile을 기반으로 이미지 생성
                    sh "docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} ."
                    sh "docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Docker Image Push') {
            steps {
                echo "🚀 Docker Hub로 이미지 업로드 중..."
                // 젠킨스에 등록한 Credentials ID('dockerhub-credentials')를 사용해 안전하게 로그인
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
                echo "🧹 WSL 호스트 서버 용량 관리를 위해 빌드에 쓴 로컬 이미지 삭제..."
                sh "docker rmi ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
                sh "docker rmi ${DOCKERHUB_USER}/${IMAGE_NAME}:latest"
            }
        }
    }
}