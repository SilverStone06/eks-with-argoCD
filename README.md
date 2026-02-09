# EKS + ArgoCD CI/CD

FastAPI/Nginx 애플리케이션을 Docker로 빌드해 AWS ECR에 푸시하고, GitOps(Argo CD)로 EKS에 배포하는 리포지토리입니다.

## 구성
- `fastapi/`: FastAPI 애플리케이션
- `nginx/`: 정적 페이지 + `/api/` 프록시 Nginx 설정
- `k8s/`: Service/Ingress(ALB)/Deployment 매니페스트
- `.github/workflows/`: ECR 빌드/푸시 및 매니페스트 갱신 파이프라인

## 워크플로우
- `argocd-ci.yaml`: ECR 로그인 → 이미지 빌드/푸시(커밋 SHA, latest) → `k8s/*.yaml` 이미지 태그 갱신 후 커밋/푸시

## 필요 시크릿
- `AWS_ACCESS_KEY_ID`: 배포용 IAM 사용자의 액세스 키 ID
- `AWS_SECRET_ACCESS_KEY`: 배포용 IAM 사용자의 시크릿 액세스 키

## 주의사항
- CI가 `k8s/*.yaml`을 자동 커밋/푸시하므로 원격이 앞서는 경우가 발생합니다.
- push 전에 `git pull --rebase`를 권장합니다.
- `git pull --no-rebase`는 머지 커밋을 만들 수 있어 권장하지 않습니다.
- AWS 리전은 `ap-southeast-2` 기준으로 작성되어 있습니다.
