# Cloud Development Kit

## General

* There exist 3 levels of constructs:
    - L1: Automatically generated
    - L2: High-level abstractions
    - L3: AWS solution service provided blueprints
* Constructs are buildings blocks for stacks
* Stacks are meant for deployment - use different stacks for dev/test/prod
* 1 App can contain multiple stacks
* Use nested stacks to get around 500 resources limit when deploying a single stack
* Explicitly define the environment for production stacks
* Doing so is not only saf(er) but makes it easy to query information about AWS accounts
* Resources can be used across different stacks
* Certain AWS services support removal policies (S3, ECR repos)
* Make sure to explicitly define them, especially in production environment,
* Use _tokens_ to refer to resources
* Use _context values_ instead of _parameters_
* Add tags to stacks using the _Tags_ class
* Common tags include:
    - owner
    - project
    - cost-center
    - environment (prod, test, dev)
    - [type](https://docs.aws.amazon.com/index.html)
* You can define local files/directories as assets, but may use the methods of constructs instead
* Let CDK manage roles and security groups and do so using constructs' methods (grant)
* Don't change the _logical_ id of resources
* Specify as few construct names as possible
* Many constructs support generating CloudWatch metrics - make use of them
* Use CDK pipelines to synth your stack
* Commit CDK json files to source control
* Follow [best practices to structure Python applications for CDK](https://aws.amazon.com/blogs/developer/recommended-aws-cdk-project-structure-for-python-applications/)