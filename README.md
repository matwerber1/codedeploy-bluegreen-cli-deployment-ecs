# Purpose
Show an example AWS CodeDeploy CLI command needed to trigger a deployment of the ECS Blue/Green deployment strategy. 

# Pre-requisites

1. AWS CLI installed and configured.
2. An existing ECS Service configured for Blue/Green Deployment. 

# Example

Update the application names, deployment groups, etc. as needed, below:

```sh
#!/usr/bin/bash
APPSPEC=$'version: 0.0
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: arn:aws:ecs:us-east-1:123123123123:task-definition/docker-node-listener:29
        LoadBalancerInfo:
          ContainerName: 'docker-node-listener'
          ContainerPort: 80
'

aws deploy create-deployment \
--application-name "AppECS-FargateCluster-fargate-alb-http" \
--deployment-group-name "DgpECS-FargateCluster-fargate-alb-http" \
--revision revisionType=String,string={content="$APPSPEC"}
```