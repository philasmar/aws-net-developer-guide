--------

Do you want to deploy your \.NET applications to AWS in just a few simple clicks? Try our new [\.NET CLI tooling](https://www.nuget.org/packages/AWS.Deploy.CLI/) for a simplified deployment experience\! Read our [blog post](https://aws.amazon.com/blogs/developer/reimagining-the-aws-net-deployment-experience/) and submit your feedback on [GitHub](https://github.com/aws/aws-dotnet-deploy)\!

 [https://github.com/aws/aws-dotnet-deploy/](https://github.com/aws/aws-dotnet-deploy/)

For additional information, see the section for the [deployment tool](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/deployment-tool.html) in this guide\.

--------

# Terminating an Amazon EC2 instance<a name="terminate-instance"></a>

When you no longer need one or more of your Amazon EC2 instances, you can terminate them\.

This example shows you how to use the AWS SDK for \.NET to terminate EC2 instances\. It takes an instance ID as input\.

## SDK references<a name="w131aac23c15c19c19c11b7b1"></a>

NuGet packages:
+ [AWSSDK\.EC2](https://www.nuget.org/packages/AWSSDK.EC2)

Programming elements:
+ Namespace [Amazon\.EC2](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/EC2/NEC2.html)

  Class [AmazonEC2Client](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/EC2/TEC2Client.html)
+ Namespace [Amazon\.EC2\.Model](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/EC2/NEC2Model.html)

  Class [TerminateInstancesRequest](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/EC2/TTerminateInstancesRequest.html)

  Class [TerminateInstancesResponse](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/EC2/TTerminateInstancesResponse.html)

```
using System;
using System.Threading.Tasks;
using System.Collections.Generic;
using Amazon.EC2;
using Amazon.EC2.Model;

namespace EC2TerminateInstance
{
  class Program
  {
    static async Task Main(string[] args)
    {
      if((args.Length == 1) && (args[0].StartsWith("i-")))
      {
        // Terminate the instance
        var ec2Client = new AmazonEC2Client();
        await TerminateInstance(ec2Client, args[0]);
      }
      else
      {
        Console.WriteLine("\nCommand-line argument missing or incorrect.");
        Console.WriteLine("\nUsage: EC2TerminateInstance instance-ID");
        Console.WriteLine("  instance-ID - The EC2 instance you want to terminate.");
        return;
      }
    }

    //
    // Method to terminate an EC2 instance
    private static async Task TerminateInstance(IAmazonEC2 ec2Client, string instanceID)
    {
      var request = new TerminateInstancesRequest{
        InstanceIds = new List<string>() { instanceID }};
      TerminateInstancesResponse response =
        await ec2Client.TerminateInstancesAsync(new TerminateInstancesRequest{
          InstanceIds = new List<string>() { instanceID }
        });
      foreach (InstanceStateChange item in response.TerminatingInstances)
      {
        Console.WriteLine("Terminated instance: " + item.InstanceId);
        Console.WriteLine("Instance state: " + item.CurrentState.Name);
      }
    }
  }
}
```

After you run the example, it's a good idea to sign in to the [Amazon EC2 console](https://console.aws.amazon.com/ec2/) to verify that the [EC2 instance](https://console.aws.amazon.com/ec2/v2/home#Instances) has been terminated\.