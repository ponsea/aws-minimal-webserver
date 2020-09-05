# To create a stack of CloudFormation
Create SSH key pair to access EC2 instance in advance.
If `SshLocation` parameter is omitted, it will be '0.0.0.0/0' (no limit).
```
aws cloudformation create-stack \
  --stack-name minimal-webserver \
  --template-body file://template.yaml \
  --parameters \
      ParameterKey=Ec2InstanceType,ParameterValue=t2.micro \
      ParameterKey=SshKeyName,ParameterValue=<YOUR SSH KEY NAME> \
      ParameterKey=SshLocation,ParameterValue=<YOUR SSH LOCATION>
```

# To get the public IP of your EC2 instance and the endpoint of ApiGateway
Make sure the stack status is `CREATE_COMPLETE`.
```
aws cloudformation describe-stacks --stack-name minimal-webserver
{
    "Stacks": [
        {
            ...

            "StackStatus": "CREATE_COMPLETE"

            ...

            "Outputs": [
                {
                    "OutputKey": "ApiGatewayUrl",
                    "OutputValue": "https://i9jc3zfetl.execute-api.ap-northeast-1.amazonaws.com/",
                    "Description": "API Gateway endpoint URL for $default stage"
                },
                {
                    "OutputKey": "Ec2InstancePublicIp",
                    "OutputValue": "18.181.180.251",
                    "Description": "EC2 Instance Public IP"
                }
            ],

            ...
    ]
}
```

# To access the EC2 instance using SSH.
```
ssh -i "/path/to/your-ssh-key.pem" ec2-user@<YOUR EC2 PUBLIC IP>
```
