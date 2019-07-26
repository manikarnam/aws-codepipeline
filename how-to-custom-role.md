# Manage the CodePipeline Service Role<a name="how-to-custom-role"></a>

Permissions for some aspects of the pipeline execution process are granted to another role type that acts on behalf of CodePipeline, rather than to IAM users\. The **Service role ** is an IAM role that gives CodePipeline permission to use resources in your account\. Service role creation is only required the first time you create a pipeline in CodePipeline\.

CodePipeline uses this service role when processing revisions through the stages and actions in a pipeline\. That role is configured with one or more policies that control access to the AWS resources used by the pipeline\. You might want to attach additional policies to this role, edit the policy attached to the role, or configure policies for other service roles in AWS\. You might also want to attach a policy to a role when configuring cross\-account access to your pipeline\.

**Important**  
Modifying a policy statement or attaching another policy to the role can prevent your pipelines from functioning\. Be sure that you understand the implications before you modify the service role for CodePipeline in any way\. Make sure you test your pipelines after making any changes to the service role\.

**Topics**
+ [Review the Default CodePipeline Service Role Policy](#view-default-service-role-policy)
+ [Add Permissions for Other AWS Services](#how-to-update-role-new-services)
+ [Remove Permissions for Unused AWS Services](#remove-permissions-from-policy)

## Review the Default CodePipeline Service Role Policy<a name="view-default-service-role-policy"></a>

By default, the policy statement for the IAM service role for CodePipeline, AWS\-CodePipeline\-Service, includes permissions needed for CodePipeline to use other resources in your account\. 

**Note**  
In the console, service roles created before September 2018 are created with the name "oneClick\_AWS\-CodePipeline\-Service\_*ID\-Number*"\.  
Service roles created after September 2018 use the service role name format "AWSCodePipelineServiceRole\-*Region*\-*Pipeline\_Name*"\. For example, for a pipeline named MyFirstPipeline created in the console in eu\-west\-2, the service role named "AWSCodePipelineServiceRole\-eu\-west\-2\-MyFirstPipeline" is created\.

AWS\-CodePipeline\-Service currently includes the following policy statement: 

```
{
    "Statement": [
        {
            "Action": [
                "iam:PassRole"
            ],
            "Resource": "*",
            "Effect": "Allow",
            "Condition": {
                "StringEqualsIfExists": {
                    "iam:PassedToService": [
                        "cloudformation.amazonaws.com",
                        "elasticbeanstalk.amazonaws.com",
                        "ec2.amazonaws.com",
                        "ecs-tasks.amazonaws.com"
                    ]
                }
            }
        },
        {
            "Action": [
                "codecommit:CancelUploadArchive",
                "codecommit:GetBranch",
                "codecommit:GetCommit",
                "codecommit:GetUploadArchiveStatus",
                "codecommit:UploadArchive"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "codedeploy:CreateDeployment",
                "codedeploy:GetApplication",
                "codedeploy:GetApplicationRevision",
                "codedeploy:GetDeployment",
                "codedeploy:GetDeploymentConfig",
                "codedeploy:RegisterApplicationRevision"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "elasticbeanstalk:*",
                "ec2:*",
                "elasticloadbalancing:*",
                "autoscaling:*",
                "cloudwatch:*",
                "s3:*",
                "sns:*",
                "cloudformation:*",
                "rds:*",
                "sqs:*",
                "ecs:*"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "lambda:InvokeFunction",
                "lambda:ListFunctions"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "opsworks:CreateDeployment",
                "opsworks:DescribeApps",
                "opsworks:DescribeCommands",
                "opsworks:DescribeDeployments",
                "opsworks:DescribeInstances",
                "opsworks:DescribeStacks",
                "opsworks:UpdateApp",
                "opsworks:UpdateStack"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "cloudformation:CreateStack",
                "cloudformation:DeleteStack",
                "cloudformation:DescribeStacks",
                "cloudformation:UpdateStack",
                "cloudformation:CreateChangeSet",
                "cloudformation:DeleteChangeSet",
                "cloudformation:DescribeChangeSet",
                "cloudformation:ExecuteChangeSet",
                "cloudformation:SetStackPolicy",
                "cloudformation:ValidateTemplate"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "codebuild:BatchGetBuilds",
                "codebuild:StartBuild"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Effect": "Allow",
            "Action": [
                "devicefarm:ListProjects",
                "devicefarm:ListDevicePools",
                "devicefarm:GetRun",
                "devicefarm:GetUpload",
                "devicefarm:CreateUpload",
                "devicefarm:ScheduleRun"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "servicecatalog:ListProvisioningArtifacts",
                "servicecatalog:CreateProvisioningArtifact",
                "servicecatalog:DescribeProvisioningArtifact",
                "servicecatalog:DeleteProvisioningArtifact",
                "servicecatalog:UpdateProduct"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudformation:ValidateTemplate"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ecr:DescribeImages"
            ],
            "Resource": "*"
        }
    ],
    "Version": "2012-10-17"
}
```

**Note**  
Make sure your service role for CodePipeline includes the `"elasticbeanstalk:DescribeEvents"` action for any pipelines that use AWS Elastic Beanstalk\. Without this permission, AWS Elastic Beanstalk deploy actions hang without failing or indicating an error\.

## Add Permissions for Other AWS Services<a name="how-to-update-role-new-services"></a>

You must update your service role policy statement with permissions for an AWS service not already included in the default service role policy statement before you can use it in your pipelines\.

This is especially important if the service role you use for your pipelines was created before support was added to CodePipeline for an AWS service\.

The following table shows when support was added for other AWS services\. 


****  

| AWS Service | CodePipeline Support Date | 
| --- | --- | 
| Amazon ECR | November 27, 2018 | 
| AWS Service Catalog | October 16, 2018 | 
| AWS Device Farm | July 19, 2018 | 
| Amazon ECS | December 12, 2017 | 
| CodeCommit | April 18, 2016 | 
| AWS OpsWorks | June 2, 2016 | 
| AWS CloudFormation | November 3, 2016 | 
| AWS CodeBuild | December 1, 2016 | 

To add permissions for an additional supported service, follow these steps:

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the IAM console, in the navigation pane, choose **Roles**, and then choose your `AWS-CodePipeline-Service` role from the list of roles\.

1. On the **Permissions** tab, in **Inline Policies**, in the row for your service role policy, choose **Edit Policy**\.
**Note**  
Your service role has a name in a format similar to `oneClick_AWS-CodePipeline-1111222233334`\.

1. Add the required permissions in the **Policy Document** box\. For example, for CodeCommit support, add the following to your policy statement:

   ```
   {
     "Action": [  
         "codecommit:GetBranch",
         "codecommit:GetCommit",
         "codecommit:UploadArchive",
         "codecommit:GetUploadArchiveStatus",      
         "codecommit:CancelUploadArchive"
               ],
     "Resource": "*",
     "Effect": "Allow"
   },
   ```

   For AWS OpsWorks support, add the following to your policy statement:

   ```
   {
       "Action": [
           "opsworks:CreateDeployment",
           "opsworks:DescribeApps",
           "opsworks:DescribeCommands",
           "opsworks:DescribeDeployments",
           "opsworks:DescribeInstances",
           "opsworks:DescribeStacks",
           "opsworks:UpdateApp",
           "opsworks:UpdateStack"
       ],
       "Resource": "*",
       "Effect": "Allow"
   },
   ```

   For AWS CloudFormation support, add the following to your policy statement:

   ```
   {
       "Action": [
           "cloudformation:CreateStack",
           "cloudformation:DeleteStack",
           "cloudformation:DescribeStacks",
           "cloudformation:UpdateStack",
           "cloudformation:CreateChangeSet",
           "cloudformation:DeleteChangeSet",
           "cloudformation:DescribeChangeSet",
           "cloudformation:ExecuteChangeSet",
           "cloudformation:SetStackPolicy",
           "cloudformation:ValidateTemplate",
           "iam:PassRole"
       ],
       "Resource": "*",
       "Effect": "Allow"
   },
   ```

   For AWS CodeBuild support, add the following to your policy statement:

   ```
   {
       "Action": [
           "codebuild:BatchGetBuilds",
           "codebuild:StartBuild"
       ],
       "Resource": "*",
       "Effect": "Allow"
   },
   ```

   For AWS Device Farm support, add the following to your policy statement:

   ```
   {
       "Action": [
           "devicefarm:ListProjects",
           "devicefarm:ListDevicePools",
           "devicefarm:GetRun",
           "devicefarm:GetUpload",
           "devicefarm:CreateUpload",
           "devicefarm:ScheduleRun"
       ],
       "Resource": "*",
       "Effect": "Allow"
   },
   ```

   For AWS Service Catalog support, add the following to your policy statement:

   ```
   {
       "Effect": "Allow",
       "Action": [
           "servicecatalog:ListProvisioningArtifacts",
           "servicecatalog:CreateProvisioningArtifact",
           "servicecatalog:DescribeProvisioningArtifact",
           "servicecatalog:DeleteProvisioningArtifact",
           "servicecatalog:UpdateProduct"
       ],
       "Resource": "*"
   },
   {
       "Effect": "Allow",
       "Action": [
           "cloudformation:ValidateTemplate"
       ],
       "Resource": "*"
   }
   ```

1. For Amazon ECR support, add the following to your policy statement:

   ```
   {
       "Action": [
           "ecr:DescribeImages"
       ],
       "Resource": "*",
       "Effect": "Allow"
   },
   ```

1. For Amazon ECS, the following are the minimum permissions needed to create pipelines with an Amazon ECS deploy action\.

   ```
   {
       "Action": [
           "ecs:DescribeServices",
           "ecs:DescribeTaskDefinition",
           "ecs:DescribeTasks",
           "ecs:ListTasks",
           "ecs:RegisterTaskDefinition",
           "ecs:UpdateService"
       ],
       "Resource": "*",
       "Effect": "Allow"
   },
   ```
**Note**  
When you create IAM policies, follow the standard security advice of granting least privilege—that is, granting only the permissions required to perform a task\. Certain API calls support resource\-based permissions and allow access to be limited\. For example, in this case, to limit permissions when calling `DescribeTasks` and `ListTasks`, you can replace the wildcard character \(\*\) with a specific resource ARN or with a wildcard character \(\*\) in a resource ARN\.

1. Choose **Validate Policy** to ensure the policy contains no errors\. When the policy is error\-free, choose **Apply Policy**\.

## Remove Permissions for Unused AWS Services<a name="remove-permissions-from-policy"></a>

You can edit the service role statement to remove access to resources you do not use\. For example, if none of your pipelines include Elastic Beanstalk, you could edit the policy statement to remove the section that grants Elastic Beanstalk resources\. For example, you could remove the following section from the policy statement:

```
    {
    "Action": [
        "elasticbeanstalk:*",
        "ec2:*",
        "elasticloadbalancing:*",
        "autoscaling:*",
        "cloudwatch:*",
        "s3:*",
        "sns:*",
        "cloudformation:*",
        "rds:*",
        "sqs:*",
        "ecs:*",
        "iam:PassRole"
    ],
    "Resource": "*",
    "Effect": "Allow"
},
```

Similarly, if none of your pipelines includes CodeDeploy, you can edit the policy statement to remove the section that grants CodeDeploy resources:

```
    {
    "Action": [
        "codedeploy:CreateDeployment",
        "codedeploy:GetApplicationRevision",
        "codedeploy:GetDeployment",
        "codedeploy:GetDeploymentConfig",
        "codedeploy:RegisterApplicationRevision"
    ],
    "Resource": "*",
    "Effect": "Allow"
},
```

For more information about IAM roles, see [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide//roles-toplevel.html)\.