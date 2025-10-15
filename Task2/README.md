# Шаги
1. Поднимите лоĸальный ĸластер Kubernetes в Minikube.
2. Аĸтивируйте metrics-server. В Minikube это можно сделать,включив metrics-server при запуске:
```BASH
    minikube start --addons=metrics-server 
```
3. Если Minikube уже был запущен без metrics-server, его можно активировать отдельно:
```BASH
minikube addons enable metrics-server
``` 
4. Убедитесь, что metrics-server активен. Вы можете проверить статус metrics-server с помощью команды:
```BASH
kubectl get pods -n kube-system | grep metrics-server
```
5. Убедиться что метрики доступны 
```BASH
kubectl top nodes
kubectl top pods
``` 
6. Загружаем image
```BASH
docker pull ghcr.io/yandex-practicum/scaletestapp@sha256:eff20ae3ae2d596375f9e
d6d612a78d149a35a66cd2907ea90d7175ca918c993
```
7. Выполняем 
```BASH
kubectl apply -f deployment.yml
kubectl apply -f service.yml
kubectl apply -f hpa-memory.yml
```
8. проверяем 
```bash
kubectl get pods
kubectl get svc
kubectl get hpa

kubectl describe pod -l app=scaletestapp
# Для того чтобы открыть приложение в браузере
 minikube service scaletestapp-service
``` 
9. Запуск Locust
```bash
python -m venv myenv
# Command Prompt
myenv\Scripts\activate
# PowerShell
myenv\Scripts\Activate.ps1

pip install locust
```