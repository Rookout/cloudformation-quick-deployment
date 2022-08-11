## Deploy Rookout On-Prem Stack on AWS ECS Fargate Cluster using CloudFormation

This CloudFormation Stack provision Rookout Controller and Rookout On-Prem Datastore on AWS ECS Fargate cluster.
This nested stack include:
1. VPC
2. ECS Cluster
3. Application load-balancer
4. Certificate issue from ACM(Amazon Certificate Manager) for rookout subdomains
5. Rookout ETL Controller https://docs.rookout.com/docs/etl-controller-intro/
6. (Optional) Rookout On-Prem datastore component https://docs.rookout.com/docs/dop-intro/
7. (Optional) Demo application https://github.com/Rookout/tutorial-python

*For custom infrastructure deployment we are recommending our official terraform module https://registry.terraform.io/modules/rookout/rookout-deployment/aws/latest*
### The stack implements the following network architecture:

<img src="https://github.com/Rookout/terraform-aws-rookout-deployment/blob/main/documentation/AWS_Deployment_Plain_Network.jpgg" width="791" height="416">

### Prerequisites
1. AWS account
2. Rookout enterprise account token
3. Rout53 registered domain https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register-update.html
4. Rout53 public hosted zone associated with domain https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/hosted-zones-working-with.html.

### Level of rookout deployment
1. Controller only
2. Controller + Datastore
3. Controller + Datastore + Demo application (default)

### Endpoints
wss://controller.rookout.PROVIDED_DOMAIN - URL of the controller, used for SDK (rooks) .

https&#65279;://datastore.rookout.PROVIDED_DOMAIN - URL to the datastore, used with rookout client (web browser application).

https&#65279;://demo.rookout.PROVIDE_DOMAIN - Flask demo application for debuging.

### Quick Start

1. Launch Stack [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?rookout-stack=myteststack&templateURL=https://rookout-on-prem-cloudformation.s3.amazonaws.com/rookout-stack.yaml)
2. Follow the cloudformation wizard

### Generating Stack from source and upload to your own bucket

1. ```aws cloudformation package --template-file rookout-template.yaml --output-template rookout-stack.yaml --s3-bucket <YOUR BUCKET NAME>```
2. (Optional)```aws s3 cp rookout-stack.yaml s3://<YOUR BUCKET NAME>```
3. Follow the cloudformation wizard or use AWS-CLI https://docs.aws.amazon.com/cli/latest/reference/cloudformation/deploy/index.html

### Parameters

| ParameterName  | Description                                                 | Default |
| ------------- |-------------------------------------------------------------| ------------ |
|HostedZone| Public Route53 HostedZone where DNS reccords will be created ||
|Domain| Your domain assosiated with HostedZone                      ||
|RookoutToken| Rookout token                                               ||
|DataStoreDeploy| Deploy Rookout On-Prem datastore true/false                 |true|
|DemoDeploy| Deploy demo application true/false                          |true|
|ControllerCPU|The number of cpu units used by the Controller task|2048|
|ControllerMemory|The amount (in MiB) of memory used by the Controller task|4096|
|DataStoreCPU|The number of cpu units used by the DataStore task|2048|
|DataStoreMemory|The amount (in MiB) of memory used by the DataStore task|4096|

### Outputs
| OutputName  | Description |
| ------------- | ------------- |
|ControllerEndpoint|URL of the controller, used for SDK (rooks)|
|DataStoreEndpoint|URL to the datastore, used with rookout client (web browser application)|
|DemoAppURL|Flask demo application for debuging URL|

   
