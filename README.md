Problem statement
==
- Jenkins and EKS are on different VPC.
- Make sense becuase Jenkins can be common for many application clusters/Projects.
- We need to enable communication between the VPC.
- Allow traffic from Jenkins to EKS on port 443. Add inbound rules in EKS security group
- Add routes in VPC routing tables between the eks-private and default routing tables.
- Add subnet range in both default and EKS private route table
- Once done enables EKS public endpoint access, if not only the traffic inside the VPC can access the EKS.
- Install kubectl client in Jenkins agent.

```
aws eks update-cluster-config --name roboshop-dev --resources-vpc-config endpointPublicAccess=true  --region us-east-1
```

Use the same parameters
==
- Install the **rebuilder** plugin to reuse the same parameter.

Catalogue-CI -> Catalogue-deploy -> Jenkins-library
==
- Build once and run anywhere
- If first time, it will install, otherwise it will upgrade
- helm upgrade --install $component -f values-$deploy_to.yaml -n $project
- Failures can be
   - Build errors
   - Scanning related errors
   - Testing related errors
   - Deployment related errors

  - We are following multiple strategies to make our applications stable and defect free.
  * Shift left (changes only on feature branch trigger build pipeline)
  * DevSecOps  (SAST,DAST, sonarqube, dependabot, trivy, Unit test)
  * Build once and run anywhere 

STEPs for Application
==
- Create infra folder
- Create VPC pipeline, it tiggers security group pipeline as downstream
- create parallel pipeline in secuity group pipeline with dependencies
- Once all infra is created proceed with CI/CD
- Infra setup is mostly onetime, never destroy.

  
STEPs for Application
==
- Create one multibranch repo for each backend CI.
- Build global trusted shared pipeline(owned by devops) for each programing.
- JAVA, NODEJS, PYTHON
- Trigger CI pipeline only for changes in feature branch.
- The pipleline will gather, security check, quality check, build image, image scan, push image and trigger downstream deploy, only when asked.
- One normal pipeline to deploy resources in dev and perform funtional testing, it will be triggered by CI pipeline if build pipeline is successful.
- Clone the repo databases-k8s and manually deploy the four databases before deploying the backends.
- Creare ALB before deploying frontend.
