```markdown
# 깃 브랜치 테스트에 관한 설명

깃 브랜치는 독립된 작업 공간을 만들어 기능 개발, 버그 수정, 실험 등을 메인 코드와 분리해 안전하게 진행할 수 있게 합니다. 브랜치 테스트는 새 코드가 기존 코드에 영향을 주지 않는지 확인하는 과정입니다.

## 기본 워크플로우

- 현재 브랜치 확인: `git status` 또는 `git branch`
- 새 브랜치 생성: `git branch feature-branch` 또는 `git checkout -b feature-branch`
- 브랜치 전환: `git checkout feature-branch`
- 작업 후 커밋: `git add .` 그리고 `git commit -m "작업 내용"`
- 원격에 푸시: `git push -u origin feature-branch`

## 테스트 전략

- 로컬 유닛/통합 테스트 실행: 프로젝트의 테스트 명령을 사용하여 변경점이 기능을 깨뜨리지 않는지 확인합니다 (예: `pytest`, `npm test`).
- CI 연동: 원격으로 푸시하면 CI가 자동으로 빌드·테스트하도록 설정해 자동 검증을 수행합니다.
- 코드 리뷰: PR(풀 리퀘스트)을 통해 동료가 코드와 테스트 결과를 검토하도록 합니다.

## 브랜치 병합(Merge)과 충돌 처리

- 변경이 안정적이면 메인(예: `main` 또는 `master`) 브랜치로 PR을 생성합니다.
- 충돌 발생 시 로컬에서 `git pull --rebase origin main` 또는 `git merge origin/main`으로 최신 변경을 반영하고 충돌을 해결한 뒤 재커밋합니다.

## 베스트 프랙티스

- 작은 단위의 커밋과 자주 푸시
- 명확한 브랜치/커밋 메시지
- 테스트 커버리지 유지
- 긴-lived 브랜치는 정기적으로 메인 브랜치의 변경을 병합

``` 
