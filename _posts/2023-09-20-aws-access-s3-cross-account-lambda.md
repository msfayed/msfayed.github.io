---
layout: default
title: AWS Cross Account S3 Access through Lambda
date: '2023-09-20T10:00:00.000-03:00'
author: Mohammed Fayed
tags:
- AWS
- S3
- Lambda
modified_time: '2023-09-20T10:00:00.000-03:00'
---

# AWS Cross Account S3 Access through Lambda 

There are 2 ways to access S3 bucket from another account, using S3 bucket policy or Assume Role

I assume that the account number `111111111111` is holding the Lambda function, and the account number `222222222222` is holding the S3 bucket
I ssume that the S3 bukcet is encrypted with a KMS key (this is optional if S3 bucket is not encrypted)

## Bukcet Policy method


### account 111111111111, set this policy for the *Lambda execution role* :

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:*"
            ],
            "Resource": [
                "arn:aws:s3:::BUCKET_NAME",
                "arn:aws:s3:::BUCKET_NAME/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "kms:Encrypt",
                "kms:Decrypt",
                "kms:DescribeKey",
                "kms:GenerateDataKey*"
            ],
            "Resource": [
              "arn:aws:kms::222222222222:key/KEY_ID"
            ]
        }
    ]
}
```

### account 222222222222, And this for the *Bucket policy*

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::111111111111:role/lambda-execution-role"
            },
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::BUCKET_NAME",
                "arn:aws:s3:::BUCKET_NAME/*"
            ]
        }
    ]
}
```

### account 222222222222, For *KMS Key Policy*

```json
{
    "Version": "2012-10-17",
    "Statement": [      
        {
            "Effect": "Allow",
            "Action": [
                "kms:Encrypt",
                "kms:Decrypt",
                "kms:DescribeKey",
                "kms:GenerateDataKey*",
                "kms:ReEncrypt*"
            ],
            "Resource": "*",
            "Principal": { "AWS": "arn:aws:iam::111111111111:role/lambda-execution-role" }
        }
    ]
}
```

![Bukcet Policy](/assets/2023-09-20/bucket-policy.png)


### sample Lambda code

```javascript
import { S3Client, GetObjectCommand, ListObjectsCommand  } from "@aws-sdk/client-s3";
export const handler = async (event, context) => {
  const s3BucketName = "BUCKET_NAME";
  const s3FileName = "123.txt";
  const s3Client = new S3Client({
        region: "us-west-2"
    }); 
    
   // ############################
   
    const listObjectsParams = {
        Bucket:s3BucketName
    };
    
    const listObjectsCommand = new ListObjectsCommand(listObjectsParams);
    
    console.log("calling ListObjectsCommand");
    const listObjectsOutput = await s3Client.send(listObjectsCommand);
    console.log(listObjectsOutput.Contents);
   // ############################
    const getObjectCommand = new GetObjectCommand({
      Bucket: s3BucketName,
      Key: s3FileName,
    });
    
    console.log("calling GetObjectCommand");
    const response = await s3Client.send(getObjectCommand);
  
    // Process the file contents
    const fileContents = await response.Body.transformToString();
    console.log(fileContents);
  
    // ############################
  
};
```


## Assume Role

here we create a new role in account `222222222222` and assign the required policies to it then our lambda function assume that role when trying to access the S3 bucket.

### account 222222222222, set this policy for the newly created *role* :

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::BUCKET_NAME",
                "arn:aws:s3:::BUCKET_NAME/*"
            ]
        }
    ]
}
```

and also we need to add this to the role `Trusted entities`

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::111111111111:role/lambda-execution-role"
            },
            "Action": "sts:AssumeRole",
            "Condition": {}
        }
    ]
}
```


### account 111111111111, set this policy for the *Lambda execution role* :

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sts:AssumeRole"
            ],
            "Resource": [
                "arn:aws:iam::222222222222:role/S3ROLE"
            ]
        }
    ]
}
```

![assume role](/assets/2023-09-20/assume-role.png)

### sample Lambda code

```javascript
import { S3Client, GetObjectCommand, ListObjectsCommand } from "@aws-sdk/client-s3";
import { STSClient, AssumeRoleCommand } from "@aws-sdk/client-sts";

export const handler = async (event, context) => {

  const roleArn = "arn:aws:iam::222222222222:role/S3ROLE";
  const roleSessionName = "SessionName";
  const s3BucketName = "BUCKET_NAME";
  const s3FileName = "123.txt";

  // Assume the role in the different account
  const stsClient = new STSClient({ region: "us-west-2" });
  const assumeRoleCommand = new AssumeRoleCommand({
    RoleArn: roleArn,
    RoleSessionName: roleSessionName,
  });
  const { Credentials } = await stsClient.send(assumeRoleCommand);
  
  console.log(Credentials);
  
  
   const s3Client = new S3Client({
        region: "us-west-2",
        credentials: {
            accessKeyId: Credentials.AccessKeyId,
            secretAccessKey: Credentials.SecretAccessKey,
            sessionToken: Credentials.SessionToken
        }
    });
    
   // ############################
   
    const listObjectsParams = {
        Bucket:s3BucketName
    };
    
    const listObjectsCommand = new ListObjectsCommand(listObjectsParams);
    const listObjectsOutput = await s3Client.send(listObjectsCommand);
    console.log(listObjectsOutput.Contents);

   // ############################
  
  try {
    
    const getObjectCommand = new GetObjectCommand({
      Bucket: s3BucketName,
      Key: s3FileName,
    });
    const response = await s3Client.send(getObjectCommand);
  
    // Process the file contents
    // The Body object also has 'transformToByteArray' and 'transformToWebStream' methods.

    const fileContents = await response.Body.transformToString();
    console.log(fileContents);
  
  } catch (err) {
    console.error(err);
  }
  
     // ############################
  
};

```


sources:

- [Cross-account S3 access using Lambda Functions - by Tejas Gupta - Medium](https://medium.com/@2017tejasgupta/cross-account-s3-access-using-lambda-functions-14d403d5f687)
- [S3 bucket access from the same and another AWS account – Tom Gregory](https://tomgregory.com/s3-bucket-access-from-the-same-and-another-aws-account/)
- [AWS Cross Account S3 Access Through Lambda Functions - DZone](https://dzone.com/articles/aws-cross-account-s3-access-through-lambda-functio)