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

Here is what is logged in CloudTrail: 

```json
{
    "eventVersion": "1.05",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "<REDACTED>",
        "arn": "arn:aws:iam::123123123123:user/<REDACTED>",
        "accountId": "123123123123",
        "accessKeyId": "<REDACTED>",
        "userName": "<REDACTED>"
    },
    "eventTime": "2019-01-18T17:51:33Z",
    "eventSource": "codedeploy.amazonaws.com",
    "eventName": "CreateDeployment",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "3.87.212.83",
    "userAgent": "aws-cli/1.16.81 Python/2.7.14 Linux/4.14.47-56.37.amzn1.x86_64 botocore/1.12.79",
    "requestParameters": {
        "applicationName": "AppECS-FargateCluster-fargate-alb-http",
        "deploymentGroupName": "DgpECS-FargateCluster-fargate-alb-http",
        "revision": {
            "revisionType": "String",
            "string": {
                "content": "version: 0.0\nResources:\n  - TargetService:\n      Type: AWS::ECS::Service\n      Properties:\n        TaskDefinition: arn:aws:ecs:us-east-1:123123123123:task-definition/docker-node-listener:29\n        LoadBalancerInfo:\n          ContainerName: docker-node-listener\n          ContainerPort: 80"
            }
        },
        "ignoreApplicationStopFailures": false,
        "updateOutdatedInstancesOnly": false
    },
    "responseElements": {
        "deploymentId": "d-4L9QHV1RX"
    },
    "requestID": "b2220b1d-9a07-477f-b747-8bb1661a421f",
    "eventID": "e5bc95e4-4695-40dc-b4eb-14e1156f2a22",
    "eventType": "AwsApiCall",
    "recipientAccountId": "123123123123"
}
```