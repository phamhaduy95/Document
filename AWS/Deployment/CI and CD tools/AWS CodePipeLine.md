
Cần nắm rõ các thuật ngữ liên quan đến pipeline:
pipeline workflow 
stage:
action
stage transiston

#### Introduction
_AWS CodePipeline_ is a continuous integration and continuous delivery service that lets you orchestrate sequences of events using AWS and other provider services.

![[Pasted image 20251103213224.png]]


![[Pasted image 20251103213207.png]]


#### pipeline mode
CodePipeline uses the following execution modes to handle the way each execution progresses through the pipeline. For more information, see [Set or change the pipeline execution mode](https://docs.aws.amazon.com/codepipeline/latest/userguide/execution-modes.html).
- SUPERSEDED mode: A more recent execution can overtake an older one. This is the default.
- QUEUED mode: Executions are processed one by one in the order that they are queued. This requires pipeline type V2.
- PARALLEL mode: In PARALLEL mode, executions run simultaneously and independently of one another. Executions don't wait for other runs to complete before starting or finishing. This requires pipeline type V2.

#### Stage and actions


A pipeline stage can include one or more actions: build, test, deploy, and invoke. You can also create custom actions.

An _action_ defines the work to perform on the revision. You can configure pipeline actions to run in series or in parallel.

If all actions in a stage complete successfully for a revision, it passes to the next stage in the pipeline. However, ==if one action fails in the stage, the revision will not pass further through the pipeline==. At this point, the stage that contains the failed action can be retried for the same revision
##### source action
Only the first stage may include source actions.
The _source_ action defines the location where you store and update source files. Modifications to files in a source repository or archive trigger deployments to a pipeline. CodePipeLine source system options include:
- Amazon S3
- AWS CodeCommit
- GitHub
- Amazon ECR (Elastic Container Registry)
- GitLab
- Bitbucket
ta define repository name và main branch. Khi có commit mối trên branch sẽ trigger pipeline

source action luôn nằm duy nhất trên stage đầu tiên
##### build action
You use a _build_ action to define tasks such as compiling source code, running unit tests, and performing other tasks that produce output artifacts for later use in your pipeline.

For example, you can use a build stage to import large assets that are not part of a source bundle into the artifact to deploy it to EC2 instances, or to compile a Java application into a JAR file. For build actions, CodePipeLine supports AWS CodeBuild and Jenkins.

##### test action
You can use _test_ actions to run various tests against source and compiled code, such as `lint` or syntax tests on source code, and unit tests on compiled, running applications


##### deploy action
The _deploy_ action is responsible for taking compiled or prepared assets and installing them on instances, on-premises servers, serverless functions, or deploying and updating infrastructure using CloudFormation templates. The following services are supported as deploy actions:

- AWS CloudFormation
- AWS CodeDeploy
- Amazon Elastic Container Service
- AWS Elastic Beanstalk


##### Approval action
An _approval_ action is a manual gate that controls whether a revision can proceed to the next stage in a pipeline. Further progress by a revision is halted until a manual approval by an AWS IAM user or IAM role occurs

he `codepipeline:PutApprovalResult` action must be included in the AWS IAM policy.

Invoke actions allow you to execute external actions via AWS Lambda or Step Functions, which allows arbitrary code to be run as part of the pipeline execution. Uses for custom actions in your pipeline can include the following:

- Backing up data volumes, Amazon S3 buckets, or databases
- Interacting with third-party products, such as posting messages to Slack channels
- Running through test interactions with deployed web applications, such as executing a test transaction on a shopping site
- Updating IAM roles to allow permissions to newly created resources



#### artifact definition 
_Artifacts_ are files that pass between actions and stages in a pipeline to provide a final result or version of the files

![[Pasted image 20251103213143.png]]


- **Understand how revisions can move through a pipeline.**  Revisions move automatically between stages in a pipeline, provided that all actions in the preceding stage complete. If a manual approval is required, the revision will not proceed until an authorized user allows it to do so. When two changes are pushed to a source repository in a short time span, the latest of the two changes will proceed through the pipeline.
 Transitions between stages can be automatic or require manual approval by an authorized user

ta có thể customize được stages với action và rule set cụ thể

the default stages used in AWS CodePipeLine Wizard
![[Pasted image 20251103220402.png]]Trigger pipeline thông qua 
Webhook từ các third-party provider như, version 1 sẽ detect theo commit mới được push
webhook v2 dựa vào git tags mới


You can roll back a stage to an execution that was successful in that stage. You can preconfigure a stage for rollback on failure, or you can manually roll back a stage. The rolled back operation will result in a new execution. The target pipeline execution chosen for rollback is used to retrieve source revisions and variables.