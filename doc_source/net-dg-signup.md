--------

Do you want to deploy your \.NET applications to AWS in just a few simple clicks? Try our new [\.NET CLI tooling](https://www.nuget.org/packages/AWS.Deploy.CLI/) for a simplified deployment experience\! Read our [blog post](https://aws.amazon.com/blogs/developer/reimagining-the-aws-net-deployment-experience/) and submit your feedback on [GitHub](https://github.com/aws/aws-dotnet-deploy)\!

 [https://github.com/aws/aws-dotnet-deploy/](https://github.com/aws/aws-dotnet-deploy/)

For additional information, see the section for the [deployment tool](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/deployment-tool.html) in this guide\.

--------

# Create an AWS account<a name="net-dg-signup"></a>

To use the AWS SDK for \.NET to access AWS services, you need an AWS account and AWS credentials\.

1. **Create an account\.**

   To create an AWS account, see [How do I create and activate a new Amazon Web Services account?](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account)

1. **Create an administrative user\.**

   Avoid using your root user account \(the initial account you create\) to access the management console and services\. Instead, create an administrative user account, as explained in [Creating your first IAM admin user and group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html)\.

   After you create the administrative user account and record the login details, **sign out of your root user account** and sign back in using the administrative account\.

If you need to close your AWS account, see [Closing an account](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/close-account.html)\.

## Next step<a name="net-dg-signup-next"></a>

[Install and configure your toolchain](net-dg-dev-env.md)

## Additional information<a name="w131aac11c13c11"></a>

For additional information about how to handle certificates and security, see [IAM best practices and use cases](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAMBestPracticesAndUseCases.html) in the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)\.

For information about adding AWS credentials to your applications, see [Configure AWS credentials](net-dg-config-creds.md)\.