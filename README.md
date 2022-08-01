## This projects deploy Tanzu cluster in your AWS Account
![image info](./Docs/Arch.png)
## Deployment steps

First authenticate to aws console.   
Then click on Launch stack button. 
You'll be redirected to a cloudformation Form to select your infra details. 
Choose the region you want to operate in prior to fill the form. The only prerequisite is to have a valid EC2 keypair and a working VPC/subnet with internet access in this region. Default VPC is ok to be used.
The project will automatically:  
1. Deploy a dedicated jumpbox
2. Download tanzu binaries from my staging bucket
3. Download Open source tools for configuration
4. Deploy a new VPC with a Tanzu managemnt Cluster and a Tanzu workload cluster inside it.   
[<img src="https://docs.cloudbolt.io/resources/Storage/cloudbolt-csmp-latest/screenshots/launch-stack.png" width="200">](https://eu-west-1.console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/quickcreate?templateURL=https://spu-tanzu-binaries.s3.eu-west-1.amazonaws.com/TKG1.4.3/TKG.yml)

