## This projects deploy Tanzu cluster in your AWS Account
![image info](./Docs/Arch.png)
## Prerequisites
Prior to testing the deployment please have a look to the configuration elements you need to prepare.  
| Name          | Description | Type           | Default  |
| ------------- |:-------------:| -----:|-----:|
| Ec2InstanceAmi| The AMI to use when creating the EC2 instance. | `AWS::EC2::Image::Id`          | the parameter store value for latest ubuntu jammy |
| SubnetId | Subnet used to launch bootstrap instance. Form will be populated with subnets available in the AWS region | `AWS::EC2::Subnet::Id` | N/A
| VPCId | VPC used to launch bootstrap instance. Form will be populated with VPC available in the AWS region | `AWS::EC2::VPC::Id` | N/A
| S3DeliveryBucketName | Name of the existing S3 bucket used to deliver Tanzu binaries. Leave default, this bucket has been made public to allow you to download templates and artifacts | `string` | spu-tanzu-binaries|
| SSHKeyName | Name of the ssh key used to access ec2 hosts. Set this up ahead of time . Form will be populated with the Keypairs names available in the AWS region| `AWS::EC2::KeyPair::KeyName` | N/A
|SSHCidrBlock| Source CIDR allowed to SSH the bootstrap server. To allow only one ip, dont forget to use /32 netmask | `string` | 0.0.0.0/0
|CreateIAMPrereqs| Whether I should create IAM resources. For a first deployment leave default. Be careful as these IAM roles have already been deployed if you used TMC on this account in the past. IAM service is global so overlaping with previous deployments, even in other regions, can occur | `bool`| true
| TKGMgmtName | the name of the management cluster to deploy | `string` | spu-tkg-mgmt
| CreateWorkloadCLuster | Whether I should create a first workload cluster |`bool`| false
| TKGWorkName | the name of the workload cluster to deploy | `string` | spu-tkg-work-01
| WorkerType | Workers instance type for your workload clusters. | `string` | m5.large
| WorkerCount | Workers instance count for your workload clusters. | `Number` | 1
| TMCEnrollCLusters | Whether I should enroll created clusters to TMC |`bool`| false
| TmcApiToken | the name of the secret inside secret manager containing your TMC API TOKEN stored in the [Plaintext format](https://docs.aws.amazon.com/secretsmanager/latest/userguide/create_secret.html)| `string` | TMC_API_TOKEN
| TmcClusterGroup | the name of the target cluster group in TMC (must exist) | `string` | spu-demo
| GitopsEnrollWorkloadCLuster | Whether I should enroll the workload cluster to a fluxcd fleet management repo |`bool`| false
| GitopsSecret | the name of the secret inside secret manager containing your git username,repo and token stored in  [JSON format](https://docs.aws.amazon.com/secretsmanager/latest/userguide/create_secret.html) with the JSON dict GITHUB_USER, GITHUB_TOKEN, GITHUB_REPO. It will be used to [bootstrap your cluster](https://fluxcd.io/docs/cmd/flux_bootstrap_github/) via CLI | `string` | fluxcd_token

## Deployment steps

First authenticate to aws console.   
Then click on Launch stack button. 
You'll be redirected to a cloudformation Form to select your infra details. 
Choose the region you want to operate in prior to fill the form. The only prerequisite is to have a valid EC2 keypair and a working VPC/subnet with internet access in this region. Default VPC is ok to be used.
The project will automatically:  
1. Deploy a dedicated jumpbox
2. Download tanzu binaries from my staging bucket
3. Download Open source tools for configuration
4. Deploy a new VPC with a Tanzu management Cluster and a Tanzu workload cluster inside it.   
5. Enroll deployed clusters to Tanzu Mission Control
6. Grab configuration of additional services with fluxCD

[<img src="https://docs.cloudbolt.io/resources/Storage/cloudbolt-csmp-latest/screenshots/launch-stack.png" width="200">](https://eu-west-1.console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/quickcreate?templateURL=https://spu-tanzu-binaries.s3.eu-west-1.amazonaws.com/TKG1.5.4/TKG.yml)

