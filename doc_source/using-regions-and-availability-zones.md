--------

Do you want to deploy your \.NET applications to AWS in just a few simple clicks? Try our new [\.NET CLI tooling](https://www.nuget.org/packages/AWS.Deploy.CLI/) for a simplified deployment experience\! Read our [blog post](https://aws.amazon.com/blogs/developer/reimagining-the-aws-net-deployment-experience/) and submit your feedback on [GitHub](https://github.com/aws/aws-dotnet-deploy)\!

 [https://github.com/aws/aws-dotnet-deploy/](https://github.com/aws/aws-dotnet-deploy/)

For additional information, see the section for the [deployment tool](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/deployment-tool.html) in this guide\.

--------

# Seeing your Amazon EC2 Regions and Availability Zones<a name="using-regions-and-availability-zones"></a>

Amazon EC2 is hosted in multiple locations worldwide\. These locations are composed of Regions and Availability Zones\. Each Region is a separate geographic area that has multiple, isolated locations known as Availability Zones\.

Read more about Regions and Availability Zones in the [Amazon EC2 User Guide for Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) or the [Amazon EC2 User Guide for Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2Win_Infrastructure.html#EC2Win_Regions)\.

This example shows you how to use the AWS SDK for \.NET to get details about the Regions and Availability Zones related to an EC2 client\. The application displays lists of the Regions and Availability Zones available to an EC2 client\.

## SDK references<a name="w131aac23c15c19c17b9b1"></a>

NuGet packages:
+ [AWSSDK\.EC2](https://www.nuget.org/packages/AWSSDK.EC2)

Programming elements:
+ Namespace [Amazon\.EC2](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/EC2/NEC2.html)

  Class [AmazonEC2Client](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/EC2/TEC2Client.html)
+ Namespace [Amazon\.EC2\.Model](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/EC2/NEC2Model.html)

  Class [DescribeAvailabilityZonesResponse](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/EC2/TDescribeAvailabilityZonesResponse.html)

  Class [DescribeRegionsResponse](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/EC2/TDescribeRegionsResponse.html)

  Class [AvailabilityZone](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/EC2/TAvailabilityZone.html)

  Class [Region](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/EC2/TRegion.html)

```
using System;
using System.Threading.Tasks;
using Amazon.EC2;
using Amazon.EC2.Model;

namespace EC2RegionsAndZones
{
  class Program
  {
    static async Task Main(string[] args)
    {
      Console.WriteLine(
        "Finding the Regions and Availability Zones available to an EC2 client...");

      // Create the EC2 client
      var ec2Client = new AmazonEC2Client();

      // Display the Regions and Availability Zones
      await DescribeRegions(ec2Client);
      await DescribeAvailabilityZones(ec2Client);
    }


    //
    // Method to display Regions
    private static async Task DescribeRegions(IAmazonEC2 ec2Client)
    {
      Console.WriteLine("\nRegions that are enabled for the EC2 client:");
      DescribeRegionsResponse response = await ec2Client.DescribeRegionsAsync();
      foreach (Region region in response.Regions)
        Console.WriteLine(region.RegionName);
    }


    //
    // Method to display Availability Zones
    private static async Task DescribeAvailabilityZones(IAmazonEC2 ec2Client)
    {
      Console.WriteLine("\nAvailability Zones for the EC2 client's region:");
      DescribeAvailabilityZonesResponse response =
        await ec2Client.DescribeAvailabilityZonesAsync();
      foreach (AvailabilityZone az in response.AvailabilityZones)
        Console.WriteLine(az.ZoneName);
    }
  }
}
```