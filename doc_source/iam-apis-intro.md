--------

Do you want to deploy your \.NET applications to AWS in just a few simple clicks? Try our new [\.NET CLI tooling](https://www.nuget.org/packages/AWS.Deploy.CLI/) for a simplified deployment experience\! Read our [blog post](https://aws.amazon.com/blogs/developer/reimagining-the-aws-net-deployment-experience/) and submit your feedback on [GitHub](https://github.com/aws/aws-dotnet-deploy)\!

 [https://github.com/aws/aws-dotnet-deploy/](https://github.com/aws/aws-dotnet-deploy/)

For additional information, see the section for the [deployment tool](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/deployment-tool.html) in this guide\.

--------

# Accessing AWS Identity and Access Management \(IAM\) with the AWS SDK for \.NET<a name="iam-apis-intro"></a>

The AWS SDK for \.NET supports [AWS Identity and Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/), which is a web service that enables AWS customers to manage users and user permissions in AWS\.

An AWS Identity and Access Management \(IAM\) *user* is an entity that you create in AWS\. The entity represents a person or application that interacts with AWS\. For more information about IAM users, see [IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html) and [IAM and STS Limits](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_iam-limits.html) in the IAM User Guide\.

You grant permissions to a user by creating a IAM *policy*\. The policy contains a *policy document* that lists the actions that a user can perform and the resources those actions can affect\. For more information about IAM policies, see [Policies and Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.

## APIs<a name="w131aac23c15c21c11"></a>

The AWS SDK for \.NET provides APIs for IAM clients\. The APIs enable you to work with IAM features such as users, roles, and access keys\.

This section contains a small number of examples that show you the patterns you can follow when working with these APIs\. To view the full set of APIs, see the [AWS SDK for \.NET API Reference](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/) \(and scroll to "Amazon\.IdentityManagement"\)\.

This section also contains [an example](net-dg-hosm.md) that shows you how to attach an IAM role to Amazon EC2 instances to make managing credentials easier\.

The IAM APIs are provided by the [AWSSDK\.IdentityManagement](https://www.nuget.org/packages/AWSSDK.IdentityManagement) NuGet package\.

## Prerequisites<a name="w131aac23c15c21c13"></a>

Before you begin, be sure you have [set up your environment](net-dg-setup.md)\. Also review the information in [Setting up your project](net-dg-config.md) and [SDK features](net-dg-sdk-features.md)\.

## Topics<a name="w131aac23c15c21c15"></a>

**Topics**
+ [APIs](#w131aac23c15c21c11)
+ [Prerequisites](#w131aac23c15c21c13)
+ [Topics](#w131aac23c15c21c15)
+ [Creating users](iam-users-create.md)
+ [Deleting users](iam-users-delete.md)
+ [Creating managed policies from JSON](iam-policies-create-json.md)
+ [Displaying policy documents](iam-policies-display.md)
+ [Granting access with a role](net-dg-hosm.md)