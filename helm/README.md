## Kubernetes Demo - Part 1
Containers included:
  - Migrator
  - Subscriber
  - SQLServer (deafult)
  - RabbitMQ (default)
 
This helm chart is the result of converting https://github.com/codaglobal/container-rfp-demo/blob/master/src/docker-compose.yml 

Additional resources:
 - Installing Docker Desktop for Windows: https://hub.docker.com/editions/community/docker-ce-desktop-windows
 - Installing kubectl: https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-linux 
 - Installing minikube: https://kubernetes.io/docs/tasks/tools/install-minikube/
 - Installing Helm & Tiller: https://helm.sh/docs/using_helm/
    - recommend using the choco install option for Windows
    - Tiller is installed at the bottom of that page
  - Run the Helm chart, from the root of the repo: `helm install --name demo .\helm --set service.type=NodePort`
  - Check the Helm deployment status:
    - `helm list --all`
    - `helm list demo`
  - Check the Kubernetes service/deployment/pod status:
    - `kubectl get services`
        - Services are available via minikube's IP address:<service port>.  For example, http://192.168.13.21:30429/
    - `kubectl get deployments`
    - `kubectl get pods`
  - Get minikube IP (to access deployed services)
    - `minikube ip`
  - To scale down part of the deployment
    - `kubectl scale --replicas=0 deployment/sqlserver`
    - `kubectl scale --replicas=0 deployment/subscriber`
    - `kubectl scale --replicas=0 deployment/rabbitmq`
    - `kubectl scale --replicas=0 deployment/migrator`
  - To delete the deployment: `helm delete --name demo`
