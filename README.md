minikube start
minikube status
minikube addons enable ingress

>> kubectl apply -f echo-server.yaml
>> kubectl apply -f echo-server2.yaml
>> kubectl apply -f httpd-deployment.yaml
>> kubectl apply -f httpd-service.yaml
>> kubectl apply -f ingress.yaml

curl.exe http://192.168.49.2/app
>>
curl: (28) Failed to connect to 192.168.49.2 port 80 after 21024 ms: Could not connect to server

kubectl get pods -n application
>>
NAME                            READY   STATUS    RESTARTS   AGE
echo-server-7fcc98d9b7-2wdp8    1/1     Running   1          31m
httpd-server-6ddccccd46-k24hw   1/1     Running   0          10m

kubectl get ingress -n application
>>
NAME           CLASS   HOSTS   ADDRESS        PORTS   AGE
test-ingress   nginx   *       192.168.49.2   80      29m

kubectl get pods -n ingress-nginx
>>
NAME                                        READY   STATUS      RESTARTS      AGE
ingress-nginx-admission-create-8pqrl        0/1     Completed   0             35m
ingress-nginx-admission-patch-g56pk         0/1     Completed   0             35m
ingress-nginx-controller-56d7c84fd4-jql5v   1/1     Running     1 (13m ago)   35m

 kubectl port-forward svc/httpd-server-service 8081:80 -n application
>>
Forwarding from 127.0.0.1:8081 -> 80
Forwarding from [::1]:8081 -> 80
Handling connection for 8081

kubectl exec -n application -it deploy/httpd-server -- wget -qO- http://localhost
>>
<html><body><h1>It works!</h1></body></html>

minikube service -n application httpd-server-service
>>
|-------------|----------------------|-------------|--------------|
|  NAMESPACE  |         NAME         | TARGET PORT |     URL      |
|-------------|----------------------|-------------|--------------|
| application | httpd-server-service |             | No node port |
|-------------|----------------------|-------------|--------------|
* service application/httpd-server-service has no node port
! Services [application/httpd-server-service] have type "ClusterIP" not meant to be exposed, however for local development minikube allows you to access this !
* Starting tunnel for service httpd-server-service.
|-------------|----------------------|-------------|------------------------|
|  NAMESPACE  |         NAME         | TARGET PORT |          URL           |
|-------------|----------------------|-------------|------------------------|
| application | httpd-server-service |             | http://127.0.0.1:58093 |
|-------------|----------------------|-------------|------------------------|
* Opening service application/httpd-server-service in default browser...
! Because you are using a Docker driver on windows, the terminal needs to be open to run it.
