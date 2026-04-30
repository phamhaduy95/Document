
CodeDeploy standardizes and automates deployments of any type of content or configuration to EC2 instances, on-premises servers, or Lambda functions

Should deployments fail in your environment, you can configure CodeDeploy with a predetermined failure tolerance. Once this tolerance is breached, deployment will automatically roll back to the last version that works.



code deploy có thể deploy code được lưu trong online repo hoặc S3
Với code được build tại local ta nên package lại trong 1file  zip và store lên s3

#### CodeDeploy component
A _revision_ is an artifact that contains both application files to deploy and an AppSpec configuration file. Application files can include source code, compiled libraries, configuration files, installation packages, static media, and other content. Similar to CodeBuild’s BuildSpec file, the AppSpec file specifies what steps CodeDeploy will follow when it performs deployments of an individual revision


https://docs.aws.amazon.com/codedeploy/latest/userguide/tutorial-lambda-sam.html
When you deploy to Lambda, a revision contains only the AppSpec file. It contains information about the functions to deploy, as well as the steps to validate that the deployment was successful.

When you use GitHub or Bitbucket, the source code does not need to be a ZIP archive, as CodeDeploy will package the repository contents on your behalf. S3, however, requires a ZIP archive file.

In _in-place deployments_, any currently running code is stopped, the new application revision is deployed, and then the application is restarted. While deployment strategies such as phasing out deployments in a load-balanced fleet can minimize downtime, the in-place method does carry the potential for service interruption.


_Blue/green deployments_ publish an entirely new instance of a code revision, after which traffic shifting routes requests to the new versions according to the deployment configuration that you define. For example, to achieve blue/green deployments with EC2 as your target, AWS configures a load balancer and two target groups. CodeDeploy publishes code to instances in the inactive target group, confirms its health, and then switches the load balancer to point to the new target group



#### Deployment
A _deployment group_ is the set of compute targets for a given deployment. With EC2/on-premises deployments, this means a specific instance, group of instances, or Auto Scaling group where your code will run. When you deploy to Lambda functions, this specifies what functions will deploy new versions. For container deployments, a deployment group refers to a specific ECS service.

##### Deploy to Auto Scaling Groups
When you deploy to Auto Scaling groups, CodeDeploy will automatically run the latest successful deployment on any new instances created when the group scales out.

When you deploy to EC2/on-premises instances, you can configure either in-place or blue/green deployments.

- **In-Place Deployments**  These deployments deploy revisions on existing instances.
- **Blue/Green Deployments**  These deployments replace currently running instances with sets of newly created instances.


##### _CodeDeployDefault_

CodeDeploy provides several built-in linear deployment configurations, such as `CodeDeployDefault.LambdaLinear10PercentEvery1Minute`. With this configuration, 10 percent of traffic is routed to the new function version every minute, until all traffic is routed after 10 minutes
##### 
CodeDeploy provides a number of built-in canary-based deployment configurations, such as `CodeDeployDefault.LambdaCanary10Percent15Minutes`. If you use this deployment configuration, 10 percent of traffic shifts in the first increment and is monitored for 15 minutes. After this time period, the 90 percent of traffic that remains shifts to the new function version. You can create additional configurations as needed.
_CodeDeployDefault.OneAtATime_
he deployment fails only if all traffic routing to replacement instances fails.
The deployment configuration specifies success criteria for deployments, such as the minimum number of healthy instances that must pass health checks during the deployment proces



##### Amazon EC2/On-Premises AppSpec
When you deploy to EC2/on-premises instances, the AppSpec file defines the following:

- A mapping of files from the revision and location on the instance
- The permissions of files to deploy
- Scripts to execute throughout the life cycle of the deployment


The AppSpec file specifies scripts to execute at each stage of the deployment life cycle. These scripts must exist in the revision for CodeDeploy to call them successfully;
The CodeDeploy agent uses the `hooks` section of the AppSpec file to reference which scripts must execute at specific times in the deployment life cycle. When the deployment is at the specified stage (such as `ApplicationStop`), the CodeDeploy agent will execute any scripts in that stage in the `hooks` section of the AppSpec file. _All scripts must return an exit code of 0 to be successful_.

``` yml
version: 0.0
os: linux
files:
 - source: /
  destination: /var/www/html/WordPress
hooks:
 BeforeInstall:
  - location: scripts/install_dependencies.sh
   timeout: 300
   runas: root
 AfterInstall:
  - location: scripts/change_permissions.sh
   timeout: 300
   runas: root
 ApplicationStart:
  - location: scripts/start_server.sh
  - location: scripts/create_test_db.sh
   timeout: 300
   runas: root
 ApplicationStop:
  - location: scripts/stop_server.sh
   timeout: 300
   runas: root
```
Because your scripts may need to reference these paths, CodeDeploy makes the following special environment variables available during all phases of the deployment life cycle:

- `LIFECYCLE_EVENT`
- `DEPLOYMENT_ID`
- `APPLICATION_NAME`
- `DEPLOYMENT_GROUP_NAME`
- `DEPLOYMENT_GROUP_ID`



```
version: 0.0
Resources:
  - MyLambdaFunction:
      Type: AWS::Lambda::Function
      Properties:
        Name: my-lambda-function-name
        Alias: live
        CurrentVersion: !GetAtt MyLambdaFunction.Version
        TargetVersion: !GetAtt MyLambdaFunction.Version
Hooks:
  - BeforeAllowTraffic:
      - LambdaFunction: "BeforeAllowTrafficHook"
  - AfterAllowTraffic:
      - LambdaFunction: "AfterAllowTrafficHook"
```

#### CodeDeploy agent

The _AWS CodeDeploy agent_ is responsible for driving and validating deployments on EC2/on-premises instances, and must be installed and running for any CodeDeploy activity to succeed on an instance. The agent currently supports Amazon Linux (Amazon EC2 only), Ubuntu Server, Microsoft Windows Server, and Red Hat Enterprise Linux, and it is available as an open source repository on GitHub (`[https://github.com/aws/aws-codedeploy-agent](https://github.com/aws/aws-codedeploy-agent)``)`.



CodeDeploy does support automatic rollbacks, but this functionality is for the EC2/On-Premises and ECS compute platforms. For Lambda, rollbacks are implemented by redeploying the previous revision

#### `AppSpec.yaml` file

When you deploy an **Amazon ECS service** using **AWS CodeDeploy**, you must include an **AppSpec file** (usually named `appspec.yaml` or `appspec.yml`) that defines **how CodeDeploy updates the ECS service**.

The **`resources`** section of the AppSpec file tells CodeDeploy which ECS resources to deploy and how to map traffic during deployment.

``` yaml
ECS 
version: 0.0
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: "arn:aws:ecs:us-east-1:123456789012:task-definition/my-task:5"
        LoadBalancerInfo:
          ContainerName: "my-container"
          ContainerPort: 8080

```


if you want to run CodeDeploy on EC2 instance, you have to install code Deploy agent manually


#### Components
|Component|Description|
|---|---|
|**Application**|Logical group of deployments (e.g., “MyWebApp”)|
|**Deployment Group**|A set of EC2 instances or Lambda aliases targeted for deployment|
|**Deployment Configuration**|Defines rollout strategy (e.g., all-at-once, half-at-a-time, canary)|
|**AppSpec file**|Blueprint for what CodeDeploy should do (lifecycle hooks, scripts, etc.)|
|**Revision**|Your deployable artifact (e.g., .zip, .tar, or AppSpec in S3 or GitHub)|
|**Service Role (IAM)**|Grants CodeDeploy permission to manage resources|

#### Deployment Hook

##### EC2 environment

| Lifecycle Hook         | When It Runs                         |
| ---------------------- | ------------------------------------ |
| **BeforeInstall**      | Before files are copied              |
| **AfterInstall**       | After files are copied               |
| **ApplicationStart**   | Start or restart the application     |
| **ValidateService**    | Final health checks                  |
| **BeforeAllowTraffic** | (Lambda/ECS) Before shifting traffic |
| **AfterAllowTraffic**  | After traffic shift completes        |

#### Rollback

If a lifecycle hook or validation fails:
- CodeDeploy automatically **stops** deployment.
- For **Blue/Green or Lambda**, it **rolls back** to the previous version/alias.
- Logs are available in **CloudWatch Logs** or `/opt/codedeploy-agent/logs/` (EC2).


✅ **Exam tip:** Rollback behavior depends on the **deployment configuration** and **monitoring alarms**.
#### Exam Scenario

| Scenario                                          | Correct Answer                         |
| ------------------------------------------------- | -------------------------------------- |
| Need to deploy to EC2 fleet with minimal downtime | In-place or blue/green with CodeDeploy |
| Deployment fails validation step                  | Check `ValidateService` hook           |
| Lambda rollback required after 5% traffic failure | Use CodeDeploy canary deployment       |
| Need zero-downtime deployment                     | Blue/Green deployment                  |
| Need to trigger CodeDeploy from GitHub push       | Integrate via CodePipeline             |
| AppSpec validation error                          | Invalid YAML syntax or missing hooks   |
| Missing deployment logs                           | Check `/opt/codedeploy-agent/logs/`    |