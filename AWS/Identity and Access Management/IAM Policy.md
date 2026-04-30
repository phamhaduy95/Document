
```
{
    "Version":"2012-10-17",		 	 	 
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": [
                    "arn:aws:iam::111122223333:role/developer",
                    "arn:aws:iam::111122223333:role/Admin"
                ]
            },
            "Action": "execute-api:Invoke",
            "Resource": [
                "execute-api:/stage/GET/pets"
            ]
        }
    ]
}
```


The **AWS CLI** determines which credentials to use based on a **specific priority order** known as the _AWS credential provider chain_.  
Here’s that order (simplified):

1. **Environment variables**  
    (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`)
    
2. **Shared credentials file** (`~/.aws/credentials`)
    
3. **CLI configuration file** (`~/.aws/config`)
    
4. **Container credentials** (for ECS tasks)
    
5. **Instance profile credentials** (IAM role attached to EC2)