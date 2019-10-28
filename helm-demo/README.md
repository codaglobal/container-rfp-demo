## Kubernetes Demo - Part 1
Containers included:
  - Migrator
  - Subscriber
  - SQLServer (deafult)
  - RabbitMQ (default)
 
This helm chart is the result of converting https://github.com/codaglobal/container-rfp-demo/blob/master/src/docker-compose.yml 

When starting this helm chart, it is key to have the RabbitMQ and SQLServer containers online first.  Once those are running, increase the 'replicas' value for the Migrator from 0 to 1.  Once the migrator is done, reduces the 'replcats' back to 0.  At this point increase the Subscriber 'replicas' to 1.  

To update the 'replicas' values, it is recommended you update the templates and deploy the upgrade via helm.  The same can be achieved by using kubectl but then that creates drift between the deployment and the helm template(s).  Both commands are listed below.
The deployment templates are set with the folllowing 'replicas' values:
  - RabbitMQ: 1
  - SQLServer: 1
  - Migrator: 0
  - Subscriber: 0


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
  - Upgrade the helm deployment to the next version
    - `helm upgrade demo .\helm`
        - Notice the change in revision number on `helm list demo`
  - Rollback the helm deployment (in this case to version 4)
    - `helm rollback helm-demo 4`
        - Notice the change in revision number on `helm list demo`, the rollback is another new version number

  Deploying the remaining containers are left as an excersize to the user.

## Native Kubernetes commands 
  - Check the Kubernetes service/deployment/pod status:
    - `kubectl get services`
        - Services are available via minikube's IP address:service port.  For example, http://192.168.13.21:30429/
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


