Problem statement
==
- Jenkins and EKS are on different VPC.
- Make sense becuase Jenkins can be common for many application clusters/Projects.
- We need to enable communication between the VPC.
- Allow traffic from Jenkins to EKS on port 443.
- Add routes in VPC routing tables between the eks-private and default routing tables.
- Once done enables EKS public endpoint access, if not only the traffic inside the VPC can access the EKS.
- Install kubectl client in Jenkins agent.

```
aws eks update-cluster-config --name roboshop-dev --resources-vpc-config endpointPublicAccess=true  --region us-east-1
```

Use the same parameters
==
- Install the rebuilder plugin to reuse the same parameter.

Catalogue-CI -> Catalogue-deploy -> Jenkins-library
==
- Build once and run anywhere
- If first time, it will take install, otherwise it will upgrade
- helm upgrade --install $component -f values-$deploy_to.yaml -n $project
- Failures can be
   - Build errors
   - Scanning related errors
   - Testing related errors
   - Deployment related errors

  - We are following multiple strategies to make our applications stable and defect free.
  * Shift left
  * DevSecOps
  * Build once and run anywhere

Deployments
==
- Create one multibranch repo for all backends
- Clone the repo databases-k8s and manually deploy the four databases.
- Now launch pipelines
- 
