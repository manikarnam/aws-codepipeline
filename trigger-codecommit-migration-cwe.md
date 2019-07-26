# Configure Your Pipelines to Use Amazon CloudWatch Events for Change Detection \(CodeCommit Source\)<a name="trigger-codecommit-migration-cwe"></a>

You can now use event\-based Amazon CloudWatch Events rules as the change detection method for your pipelines with a CodeCommit repository\. Amazon CloudWatch Events detects changes in real time and then starts your pipeline\. You must update existing pipelines to use this feature\. When you create or edit a pipeline in the console, CodePipeline creates the Amazon CloudWatch Events event\-based rule for you\.

This table includes procedures for pipelines with a CodeCommit source\.


****  

| Method | Instructions | Further action required? | 
| --- | --- | --- | 
| Configuring pipelines in the console |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/codepipeline/latest/userguide/trigger-codecommit-migration-cwe.html) See [Edit a Pipeline \(Console\)](pipelines-edit.md#pipelines-edit-console)\.  |  The Amazon CloudWatch Events rule is created for you\.  No further action is required\.  | 
| Configuring pipelines in the CLI |  Use the update\-pipeline command to set the `PollForSourceChanges` parameter to `false`\. See [Edit a Pipeline \(AWS CLI\)](pipelines-edit.md#pipelines-edit-cli)\.  |  You must create the Amazon CloudWatch Events rule\. See [ Use CloudWatch Events to Start a Pipeline \(CodeCommit Source\)](triggering.md)\.  | 
| Configuring pipelines in AWS CloudFormation |  Update the AWS CloudFormation resource stack\. See [What Is Amazon CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/)\.  |  You must create the Amazon CloudWatch Events rule\. See [ Use CloudWatch Events to Start a Pipeline \(CodeCommit Source\)](triggering.md)\.  | 