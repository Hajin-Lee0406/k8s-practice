🚀 Minikube 연습 가이드
로컬 환경에서 Docker 드라이버 기반 Minikube를 이용해
ConfigMap, Secret, Deployment, Service를 구성하는 기본 실습입니다.

⸻

1️⃣ 클러스터 시작

minikube start --driver=docker

상태 확인:
minikube status
kubectl get nodes


⸻

2️⃣ 네임스페이스 생성

kubectl create namespace kube-practice
kubectl config set-context --current --namespace=kube-practice


⸻

3️⃣ ConfigMap 생성 (환경변수 설정)

📄 app-configmap.yaml

kubectl apply -f app-configmap.yaml
kubectl get configmap
kubectl describe configmap nginx-config


⸻

4️⃣ Secret 생성 (민감정보 저장)

📄 app-secret.yaml


kubectl apply -f app-secret.yaml
kubectl get secrets
kubectl describe secret app-secret


⸻

5️⃣ Deployment 생성 (Nginx + ConfigMap + Secret 사용)

📄 nginx-deployment.yaml

kubectl apply -f nginx-deployment.yaml
kubectl get deployments
kubectl get pods -o wide


⸻

6️⃣ Service 생성 (NodePort로 외부 접근)

📄 nginx-service.yaml

kubectl apply -f nginx-service.yaml
kubectl get svc
kubectl describe svc nginx-service

접속 URL 확인:

minikube service nginx-service --url -n kube-practice
# 예: http://127.0.0.1:54901


⸻

🌐 전체 트래픽 흐름

외부 클라이언트
    ↓
http://<노드IP>:30080 (NodePort)
    ↓
Service (nginx-service:80)
    ↓
로드밸런싱 (app: nginx-sample 레이블 가진 Pod들 중)
    ↓
Pod 1: nginx 컨테이너:80
Pod 2: nginx 컨테이너:80


⸻

🧹 정리 및 종료

kubectl delete namespace kube-practice
minikube stop
minikube delete


⸻

✅ 참고 명령어

명령어	설명
kubectl get all -n kube-practice	네임스페이스 내 전체 리소스 조회
kubectl logs <pod-name>	특정 Pod 로그 확인
kubectl exec -it <pod-name> -- /bin/bash	Pod 내부 접속
minikube dashboard	Minikube 웹 대시보드 실행


⸻

원하신다면 이 README에 구조도 (트래픽 흐름 다이어그램) 도 ASCII나 이미지로 추가해드릴 수 있습니다.
그려드릴까요?
