#### Nested Stack
Ta có thể import một nested stack define sẵn và include vào 1 parent stack

Ví dụ. Ta tạo 1 template riêng cho VPC, ta sẽ include và template chính. Lưu ý các nested stack cần được upload lên S3.

**VPC stack**
``` yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Nested stack that creates a VPC

Resources:
  MyVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      Tags:
        - Key: Name
          Value: MyNestedVPC

  MySubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref MyVPC
      CidrBlock: 10.0.1.0/24
      AvailabilityZone: !Select [0, !GetAZs ""]

Outputs:
  VpcId:
    Description: The ID of the created VPC
    Value: !Ref MyVPC

  SubnetId:
    Description: The ID of the created Subnet
    Value: !Ref MySubnet

```

**Main stack**
``` yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Parent template that includes a nested VPC stack and an EC2 instance

Parameters:
  KeyName:
    Type: AWS::EC2::KeyPair::KeyName
    Description: EC2 KeyPair for SSH access

Resources:
  # Nested Stack
  NetworkStack:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/my-bucket/vpc.yaml  # must be in S3
      TimeoutInMinutes: 10
```

Lưu ý: Ta chỉ cần deploy main stack là các nested stack cũng được deploy cùng. 

#### Export and Import Value

ta có thể export 1 variable từ template sang template khác

**Export Value from stack A**

``` yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Network stack that creates a VPC

Resources:
  MyVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      Tags:
        - Key: Name
          Value: ExportedVPC

Outputs:
  VPCId:
    Description: The ID of the VPC
    Value: !Ref MyVPC
    Export:
      Name: MyCompany-VPCID
```

The `Export` field publishes `VPCId` value. Ref này sẽ tồn tại độc nhất cho toàn bộ region
The export name (`MyCompany-VPCID`) must be **unique** within the region.


**Import Value in stack B**


``` yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Application stack that uses an existing VPC

Resources:
  MySubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !ImportValue MyCompany-VPCID
      CidrBlock: 10.0.1.0/24
      AvailabilityZone: !Select [0, !GetAZs ""]
      Tags:
        - Key: Name
          Value: AppSubnet
```

Dùng `!ImportValue` để import value 

Các lưu ý quan trong khi dùng tính năng import và export
- Tên của export value cần độc nhất trong 1 region
- Ta không thể xóa template chứa export value hoặc bỏ export value khi vẫn còn reference ở template khác
- Các stack phải cùng 1 regions
- Cả 2 stack cần đều được deploy



A developer is deploying a REST API using a CloudFormation template and needs to reference the newly created API endpoint in other stacks.
`AWS::Include` is for **embedding templates** or **files**,

`Ref` only works **within the same stack**

A. Include the Export property in the original template's Outputs section and use `Fn::ImportValue` in other templates.

#### Validate template

When working with large or complex templates, `validate-template` helps you:

- Detect **syntax errors** early (e.g., indentation, missing colons, invalid resource types).
- Confirm the template is **parseable** by CloudFormation.
- Avoid failed stack creation/update due to structural issues



Deletion Policy: Một số resource được tạo
Retain: resource sẽ được giữ lại khi được update
RetainExceptOnCreate: giữ lại khi update tuy nhiên sẽ bị xóa khi bị rollback


Học cách structure lại requirement.

Các cách thức mô tả 1 flow hoạt động của hệ thống.
