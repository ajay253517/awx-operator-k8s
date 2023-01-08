## Repository for AWX deployment on kubernetes. 

Follow below steps to deploy awx on kubernetes

## Step1: Deploy custom resource definition: 

Apply file `01-crd.yaml` using kubectl apply -f 01-crd.yaml, these are not namespace specific objects so we need to apply it once.

## Step2: Deploy awx operator controller: 

Based on requirement replace namespace name in below file 

- ./dev-awx/01-dev-awx-controller.yaml 

Use `kubectl apply -f ./dev-awx/01-dev-awx-controller.yaml` This will create `awx-operator-controller` once applied we can use below command to check logs.

```
kubectl get pods -n namespace-name
kubectl logs -f pod-name -c awx-manager
```
## Step3: Create awx admin credentials 

Based on requirement replace namespace name in below file.
Create a password using base64 for which we can use `echo 'password' | base64 ` replace it in password field.

- ./dev-awx/02-dev-awx-admin.yaml

We can use `kubectl apply -f ./dev-awx/02-dev-awx-admin.yaml` it will create secret.

## Step4: Create awx deployment 

Based on requirement replace namespace name in below file.
Add admin credentials secret name in the file.
Select service type and port based on requirement. 

- ./dev-awx/03-dev-awx-deploy.yaml

We can use `kubectl apply -f ./dev-awx/03-dev-awx-deploy.yaml` it will create a pod with 4 containers [redis dev-awx-demo-web dev-awx-demo-task dev-awx-demo-ee] and we can check logs using below command. 

```
kubectl get pods -n namespace
kubectl logs -f pod-name -c  dev-awx-demo-web -n namespace ## container value can be any of 4 these redis dev-awx-demo-web dev-awx-demo-task dev-awx-demo-ee
```

### Accessing URL : 

Based on the service type url can be accessible to list the service we can use below command. 

`kubectl get svc -n dev-awx`
