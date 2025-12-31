@Library('jenkins-shared-library') _

properties([
  parameters([
    string(name: 'appVersion', defaultValue: ''),
    string(name: 'deploy_to', defaultValue: 'dev')
  ])
])

def configMap = [
    project: "roboshop",
    component: "catalogue",
    acc_id: "406682759639",
    region: "us-east-1"
    appVersion: (params.appVersion),
    deploy_to: (params.deploy_to)
]

EKSDeploy(mymap)