### Prerequisites

Các điều kiện cần thiết trước khi thực hiện deploy bao gồm:
- install eb CLI.
- kiểm tra xem IAM user kết nối qua `aws configure` có permission `AWSElasticBeanstalkFullAccess` chưa.
- application code

#### deployment step by step 
**step 1:** Khởi tạo elastic beanstalk trong project root chứa application code
``` bash
eb init
```

The EB CLI will prompt you to configure the application settings: 
- **Region:** Select your desired AWS region.
- **Application Name:** Accept the default (folder name) or enter a new name.
- **Platform:** Choose the language/runtime your application uses (e.g., Python, Node.js, Docker).

step 2:
khởi tao 1 elastic beanstalk environment gồm các EC2 instance 
```
eb create
```

the `eb create` command sử dụng các configuration files lưu trong `.ebextensions` directory.

step 3: Khi có thay đổi ở code hoặc configuration. Ta có thể dùng `eb deploy` để deploy thay đổi cho beanstalk environment
``` bash
eb deploy
```
#### elbextenstions

An `.ebextensions` configuration file allows you to customize and extend your Elastic Beanstalk environment. These files are processed during the environment launch or update process, allowing you to configure almost any aspect of the AWS resources (EC2, ASG, ELB, RDS) or the software running on the instances.

**Configuration Files** (`.config` files) placed within the `.ebextensions` directory of your source bundle, which define the infrastructure and software settings for your environment using YAML or JSON format

``` yaml
option_settings:
  aws:elasticbeanstalk:application:environment:
    # Key-Value pairs for environment variables
    DB_HOST: "my-database-endpoint.rds.amazonaws.com"
    DB_USER: "myuser"
    API_KEY: "A1B2C3D4E5F6"
    ENVIRONMENT: "production"
  aws:elasticbeanstalk:container:python:
    # Example of a platform-specific setting
    WSGIPath: application.py

```


### **Best Elastic Beanstalk strategy: Blue/Green Deployment**
developer is updating an application deployed on AWS Elastic Beanstalk. The new version is incompatible with the old version.
This scenario perfectly matches a **Blue/Green Deployment** pattern.

#### **How it works**

1. **Blue environment** = current, running production version.
    
2. **Green environment** = new environment with the updated version.
    
3. Once the new environment (Green) is fully tested and healthy:
    
    - Use the **"Swap Environment URLs"** feature in Elastic Beanstalk.
        
    - The **CNAMEs** of the two environments are swapped — instantly directing traffic to the new version.
        

#### **If something goes wrong**

- Simply **swap the URLs back** — reverting traffic to the old (stable) environment with **minimal downtime**.