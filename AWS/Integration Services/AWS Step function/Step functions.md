#### Introduction

Step Functions provides a way to model and automate complex, multi-step processes and applications using a graphical workflow editor. AWS Step Functions is based on the concepts of tasks and state machines, and can track, monitor, and manage the execution of workflows. AWS Step Functions integrates with many other AWS services, including Amazon EC2, Amazon ECS, AWS Lambda, Amazon SQS, and Amazon SNS, and can be used to build scalable, reliable, and efficient applications and microservices.

developers can create a state machine with multiple states. Each workflow has checkpoints  
that maintain each workflow throughout each defined stage.


Workflows that involve branching logic, different types of failure models, and retry logic typically use an orchestrator to keep track of the state of the overall execution. Avoid using Lambda functions for this purpose, since it results in tight coupling and complex code handling routing.