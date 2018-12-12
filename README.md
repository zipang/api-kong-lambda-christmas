# api-kong-lambda-christmas

The goal of this project is to learn how to set up a kong + aws-lambda platform.
Kong act as a poweful and flexible API gateway. You can use kong communauty edition for this workshop. Then if you want to apply it to real API use cases, you should look at the entreprise edition to have all the benefits of the software.

## STEP ONE : ENVIRONMENT SETTINGS

### Configure an API Gateway with KONG

https://aws.amazon.com/marketplace/pp/B06WP4TNKL

It is necessary to accept the Terms and condiotions before proceding to the next steps.

### Terraform

Terraform is a command line tool that can execute scripts to build and deploy any infrastructure in the cloud.
The infrastructure being written as code, it can be versioned.
AWS providers can be used to configure and instanciate any Amazon Cloud service.

Setup intructions can be found here : 

[Installing Terraform](https://learn.hashicorp.com/terraform/getting-started/install.html#installing-terraform)

### Provisionning

Export environment variables : 
```sh
export AWS_ACCESS_KEY_ID="AKIAI3YZDZJOHEMDF22A"
export AWS_SECRET_ACCESS_KEY="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"
export AWS_DEFAULT_REGION="eu-west-2"
```

Inside the project dir, enter `tf/` and execute the following commands :
```sh
terraform init
terraform apply -auto-approve
```

This should resulr in the following output if successfull :

```
Apply complete! Resources: 20 added, 0 changed, 0 destroyed.

Outputs:

ec2_global_ips = [
    3.8.88.3
]
s3_arn = [
    arn:aws:s3:::kong-bucket-314d827647775cf799568685
]
s3_bucket_id = [
    kong-bucket-314d827647775cf799568685
]
s3_website_url = [
    kong-bucket-314d827647775cf799568685.s3-website.eu-west-2.amazonaws.com
]
```

In the EC2 dashboard, (after selecting the correct region : Europe Londres) the folowing services are running 

* EC2 Instance
* Kong
* DynamoDB Table


### Create a MapQuest developer account

Create an account or login : https://developer.mapquest.com/

Create an API Key for the application :


| apidays-workshop’s Keys | Value |
|------------|------------|
| Consumer Key | mBqpaCps3pGhXMF9CxbYYofsnz6kDez4 |
| Consumer Secret | NRVKAeeybjsUDinO |
| Key Issued | Tue, 12/11/2018 - 06:34 |
| Key Expires | Never |



## STEP TWO : CREATE THE MICRO SERVICES AS LAMBDA FUNCTIONS

### Create a role to access DynamoDB from a Lambda function

In EC2 Dashboard, choose IAM


### Setup the lambda functions

1. Go to aws services/lambda/functions
2. Choose Create function
3. Choose Author from scratch 
4. Create the folowing functions :

#### getAllProducts

* Runtime : Python 3.7
* Role : DynamoLambda existing role

In the lambda_function.py, copy paste the content of getAllProducts.py from [the lambda function directory](./getAllProducts/)
In Environnement variable enter :

#### function getProductDetails

#### function createDeliveriesForAddress

AWS lamdba fonction to get the products list (id and name)
<br/>Setup kong to expose a route and call the aws lambda function
<br/>[Step two](./workshop/step2/step2.md)

#### Step three: get product details function 

AWS lambda function to get a product detail
<br/>Setup kong to expose a route and call the aws lambda function
<br/>Setup front-end and activate CORS plugin at kong level
<br/>[Step three](./workshop/step3/step3.md)

#### Step four: put an order request

AWS lambda function to request for an order
<br/>Setup kong to expose a route and call the aws lambda function
<br/>[Step four](./workshop/step4/step4.md)

### Configure KONG

