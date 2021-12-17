# K8s Tech test

## Steps

1. Create a kubernetes deployments, service in a kubernetes cluster. Use a cloud cluster (EKS/AKS/GKE) or local machine (minikube)
Bonus: create a HPA

2. Deployments needs to have a secret with name KEY_SECRET

3. Mount this secret in the deployment

4. KEY_SECRET should be a volume mount within the container

4.1 KEY_SECRET mounted as an environment varialbe

## Points to note:
- For applications, use simple ones like nginx, httpd etc.
- You can use any language or open-source tool to solve this problem
- If required, write extra code on Terraform or scripts

## Criterias:

- Provide code with YAML manifest files, pipelines and any scripts used
- Clean and readable code
- Document any steps you have taken in a README.md file
- You need to use either the public cloud (AKS,EKS,GKE) or local K8s (minikube) to run the K8s YAML files
- A dedicated service account for the deployment (dont use automount deployments)
- You must ensure that there are always a minimum of 2 pods up and running
- You must use a NodePort type service for the deployment
- Only 1 pod must be unavailable during rolling update of deployment (hint: use correct percentage when using the "maxUnavailable" paramter)

## Bonus points:

- Containers used in deployment must be scanned before being deployed and if severity is high or critical; pipeline should fail
- Container in Pod, should not be running as ROOT
- Provide any code that you used to complete this task
