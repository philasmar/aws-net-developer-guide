--------

Do you want to deploy your \.NET applications to AWS in just a few simple clicks? Try our new [\.NET CLI tooling](https://www.nuget.org/packages/AWS.Deploy.CLI/) for a simplified deployment experience\! Read our [blog post](https://aws.amazon.com/blogs/developer/reimagining-the-aws-net-deployment-experience/) and submit your feedback on [GitHub](https://github.com/aws/aws-dotnet-deploy)\!

 [https://github.com/aws/aws-dotnet-deploy/](https://github.com/aws/aws-dotnet-deploy/)

For additional information, see the section for the [deployment tool](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/deployment-tool.html) in this guide\.

--------

# Creating IAM managed policies from JSON<a name="iam-policies-create-json"></a>

This example shows you how to use the AWS SDK for \.NET to create an [IAM managed policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) from a given policy document in JSON\. The application creates an IAM client object, reads the policy document from a file, and then creates the policy\.

**Note**  
For an example policy document in JSON, see the [additional considerations](#iam-policies-create-json-additional) at the end of this topic\.

The following sections provide snippets of this example\. The [complete code for the example](#iam-policies-create-json-complete-code) is shown after that, and can be built and run as is\.

**Topics**
+ [Create the policy](#iam-policies-create-json-create)
+ [Complete code](#iam-policies-create-json-complete-code)
+ [Additional considerations](#iam-policies-create-json-additional)

## Create the policy<a name="iam-policies-create-json-create"></a>

The following snippet creates an IAM managed policy with the given name and policy document\.

The example [at the end of this topic](#iam-policies-create-json-complete-code) shows this snippet in use\.

```
    //
    // Method to create an IAM policy from a JSON file
    private static async Task<CreatePolicyResponse> CreateManagedPolicy(
      IAmazonIdentityManagementService iamClient, string policyName, string jsonFilename)
    {
      return await iamClient.CreatePolicyAsync(new CreatePolicyRequest{
        PolicyName = policyName,
        PolicyDocument = File.ReadAllText(jsonFilename)});
    }
```

## Complete code<a name="iam-policies-create-json-complete-code"></a>

This section shows relevant references and the complete code for this example\.

### SDK references<a name="w131aac23c15c21c23c17b5b1"></a>

NuGet packages:
+ [AWSSDK\.IdentityManagement](https://www.nuget.org/packages/AWSSDK.IdentityManagement)

Programming elements:
+ Namespace [Amazon\.IdentityManagement](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/IAM/NIAM.html)

  Class [AmazonIdentityManagementServiceClient](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/IAM/TIAMServiceClient.html)
+ Namespace [Amazon\.IdentityManagement\.Model](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/IAM/NIAMModel.html)

  Class [CreatePolicyRequest](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/IAM/TCreatePolicyRequest.html)

  Class [CreatePolicyResponse](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/IAM/TCreatePolicyResponse.html)

### The code<a name="w131aac23c15c21c23c17b7b1"></a>

```
using System;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Amazon.IdentityManagement;
using Amazon.IdentityManagement.Model;

namespace IamCreatePolicyFromJson
{
  // = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =
  // Class to create an IAM policy with a given policy document
  class Program
  {
    private const int MaxArgs = 2;

    static async Task Main(string[] args)
    {
      // Parse the command line and show help if necessary
      var parsedArgs = CommandLine.Parse(args);
      if((parsedArgs.Count == 0) || (parsedArgs.Count > MaxArgs))
      {
        PrintHelp();
        return;
      }

      // Get the application arguments from the parsed list
      string policyName =
        CommandLine.GetArgument(parsedArgs, null, "-p", "--policy-name");
      string policyFilename =
        CommandLine.GetArgument(parsedArgs, null, "-j", "--json-filename");
      if(   string.IsNullOrEmpty(policyName)
         || (string.IsNullOrEmpty(policyFilename) || !policyFilename.EndsWith(".json")))
        CommandLine.ErrorExit(
          "\nOne or more of the required arguments is missing or incorrect." +
          "\nRun the command with no arguments to see help.");

      // Create an IAM service client
      var iamClient = new AmazonIdentityManagementServiceClient();

      // Create the new policy
      var response = await CreateManagedPolicy(iamClient, policyName, policyFilename);
      Console.WriteLine($"\nPolicy {response.Policy.PolicyName} has been created.");
      Console.WriteLine($"  Arn: {response.Policy.Arn}");
    }


    //
    // Method to create an IAM policy from a JSON file
    private static async Task<CreatePolicyResponse> CreateManagedPolicy(
      IAmazonIdentityManagementService iamClient, string policyName, string jsonFilename)
    {
      return await iamClient.CreatePolicyAsync(new CreatePolicyRequest{
        PolicyName = policyName,
        PolicyDocument = File.ReadAllText(jsonFilename)});
    }


    //
    // Command-line help
    private static void PrintHelp()
    {
      Console.WriteLine(
        "\nUsage: IamCreatePolicyFromJson -p <policy-name> -j <json-filename>" +
        "\n  -p, --policy-name: The name you want the new policy to have." +
        "\n  -j, --json-filename: The name of the JSON file with the policy document.");
    }
  }


  // = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =
  // Class that represents a command line on the console or terminal.
  // (This is the same for all examples. When you have seen it once, you can ignore it.)
  static class CommandLine
  {
    //
    // Method to parse a command line of the form: "--key value" or "-k value".
    //
    // Parameters:
    // - args: The command-line arguments passed into the application by the system.
    //
    // Returns:
    // A Dictionary with string Keys and Values.
    //
    // If a key is found without a matching value, Dictionary.Value is set to the key
    //  (including the dashes).
    // If a value is found without a matching key, Dictionary.Key is set to "--NoKeyN",
    //  where "N" represents sequential numbers.
    public static Dictionary<string,string> Parse(string[] args)
    {
      var parsedArgs = new Dictionary<string,string>();
      int i = 0, n = 0;
      while(i < args.Length)
      {
        // If the first argument in this iteration starts with a dash it's an option.
        if(args[i].StartsWith("-"))
        {
          var key = args[i++];
          var value = key;

          // Check to see if there's a value that goes with this option?
          if((i < args.Length) && (!args[i].StartsWith("-"))) value = args[i++];
          parsedArgs.Add(key, value);
        }

        // If the first argument in this iteration doesn't start with a dash, it's a value
        else
        {
          parsedArgs.Add("--NoKey" + n.ToString(), args[i++]);
          n++;
        }
      }

      return parsedArgs;
    }

    //
    // Method to get an argument from the parsed command-line arguments
    //
    // Parameters:
    // - parsedArgs: The Dictionary object returned from the Parse() method (shown above).
    // - defaultValue: The default string to return if the specified key isn't in parsedArgs.
    // - keys: An array of keys to look for in parsedArgs.
    public static string GetArgument(
      Dictionary<string,string> parsedArgs, string defaultReturn, params string[] keys)
    {
      string retval = null;
      foreach(var key in keys)
        if(parsedArgs.TryGetValue(key, out retval)) break;
      return retval ?? defaultReturn;
    }

    //
    // Method to exit the application with an error.
    public static void ErrorExit(string msg, int code=1)
    {
      Console.WriteLine("\nError");
      Console.WriteLine(msg);
      Environment.Exit(code);
    }
  }

}
```

## Additional considerations<a name="iam-policies-create-json-additional"></a>
+ The following is an example policy document that you can copy into a JSON file and use as input for this application:

  ```
  {
    "Version" : "2012-10-17",
    "Id"  : "DotnetTutorialPolicy",
    "Statement" : [
      {
        "Sid" : "DotnetTutorialPolicyS3",
        "Effect" : "Allow",
        "Action" : [
          "s3:Get*",
          "s3:List*"
        ],
        "Resource" : "*"
      },
      {
        "Sid" : "DotnetTutorialPolicyPolly",
        "Effect": "Allow",
        "Action": [
          "polly:DescribeVoices",
          "polly:SynthesizeSpeech"
        ],
        "Resource": "*"
      }
    ]
  }
  ```
+ You can verify that the policy was created by looking in the [IAM console](https://console.aws.amazon.com/iam/home#/policies)\. In the **Filter policies** drop\-down list, select **Customer managed**\. Delete the policy when you no longer need it\.
+  For more information about policy creation, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) and the [IAM JSON policy reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)