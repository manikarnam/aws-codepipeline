# Working with Pipelines in CodePipeline<a name="pipelines"></a>

To define an automated release process in AWS CodePipeline, you create a pipeline, which is a workflow construct that describes how software changes go through a release process\. A pipeline is composed of stages and actions that you configure\. 

**Note**  
When you add Build, Deploy, Test, or Invoke stages, in addition to the default options provided with CodePipeline, you can choose custom actions that you have already created for use with your pipelines\. Custom actions can be used for tasks such as running an internally developed build process or a test suite\. Version identifiers are included to help you distinguish among different versions of a custom action in the provider lists\. For more information, see [Create and Add a Custom Action in CodePipeline](actions-create-custom-action.md)\.

Before you can create a pipeline, you must first complete the steps in [Getting Started with CodePipeline](getting-started-codepipeline.md)\.

For more information about pipelines, see [CodePipeline Concepts](concepts.md), [CodePipeline Tutorials](tutorials.md), and, if you want to use the AWS CLI to create a pipeline, [CodePipeline Pipeline Structure Reference](reference-pipeline-structure.md)\. To view a list of pipelines, see [View Pipeline Details and History in CodePipeline](pipelines-view.md)\.

**Topics**
+ [Start a Pipeline Execution in CodePipeline](pipelines-about-starting.md)
+ [Create a Pipeline](pipelines-create.md)
+ [Edit a Pipeline](pipelines-edit.md)
+ [View Pipeline Details and History](pipelines-view.md)
+ [Delete a Pipeline](pipelines-delete.md)
+ [Create a Pipeline That Uses Resources from Another Account](pipelines-create-cross-account.md)
+ [Edit a Pipeline for Push Events](update-change-detection.md)
+ [Create the CodePipeline Service Role](pipelines-create-service-role.md)
+ [Tag a Pipeline](pipelines-tag.md)