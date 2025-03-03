# Get data about the last use of an IAM access key using an AWS SDK<a name="example_iam_GetAccessKeyLastUsed_section"></a>

The following code examples show how to get data about the last use of an IAM access key\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Go ]

**SDK for Go V2**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/gov2/iam#code-examples)\. 
  

```
package main

import (
	"context"
	"flag"
	"fmt"

	"github.com/aws/aws-sdk-go-v2/config"
	"github.com/aws/aws-sdk-go-v2/service/iam"
)

// IAMGetAccessKeyLastUsedAPI defines the interface for the GetAccessKeyLastUsed function.
// We use this interface to test the function using a mocked service.
type IAMGetAccessKeyLastUsedAPI interface {
	GetAccessKeyLastUsed(ctx context.Context,
		params *iam.GetAccessKeyLastUsedInput,
		optFns ...func(*iam.Options)) (*iam.GetAccessKeyLastUsedOutput, error)
}

// WhenWasKeyUsed retrieves when an AWS Identity and Access Management (IAM) access key was last used, including the AWS Region and with which service.
// Inputs:
//     c is the context of the method call, which includes the AWS Region.
//     api is the interface that defines the method call.
//     input defines the input arguments to the service call.
// Output:
//     If successful, a GetAccessKeyLastUsedOutput object containing the result of the service call and nil.
//     Otherwise, nil and an error from the call to GetAccessKeyLastUsed.
func WhenWasKeyUsed(c context.Context, api IAMGetAccessKeyLastUsedAPI, input *iam.GetAccessKeyLastUsedInput) (*iam.GetAccessKeyLastUsedOutput, error) {
	return api.GetAccessKeyLastUsed(c, input)
}

func main() {
	keyID := flag.String("k", "", "The ID of the access key")
	flag.Parse()

	if *keyID == "" {
		fmt.Println("You must supply the ID of an access key (-k KEY-ID)")
		return
	}

	cfg, err := config.LoadDefaultConfig(context.TODO())
	if err != nil {
		panic("configuration error, " + err.Error())
	}

	client := iam.NewFromConfig(cfg)

	input := &iam.GetAccessKeyLastUsedInput{
		AccessKeyId: keyID,
	}

	result, err := WhenWasKeyUsed(context.TODO(), client, input)
	if err != nil {
		fmt.Println("Got an error retrieving when access key was last used:")
		fmt.Println(err)
		return
	}

	fmt.Println("The key was last used:", *result.AccessKeyLastUsed)
}
```
+  For API details, see [GetAccessKeyLastUsed](https://pkg.go.dev/github.com/aws/aws-sdk-go-v2/service/iam#Client.GetAccessKeyLastUsed) in *AWS SDK for Go API Reference*\. 

------
#### [ JavaScript ]

**SDK for JavaScript V3**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Get the access key\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { GetAccessKeyLastUsedCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = { AccessKeyId: "ACCESS_KEY_ID" }; //ACCESS_KEY_ID

export const run = async () => {
  try {
    const data = await iamClient.send(new GetAccessKeyLastUsedCommand(params));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-managing-access-keys.html#iam-examples-managing-access-keys-last-used)\. 
+  For API details, see [GetAccessKeyLastUsed](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/getaccesskeylastusedcommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.getAccessKeyLastUsed({AccessKeyId: 'ACCESS_KEY_ID'}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.AccessKeyLastUsed);
  }
});
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-managing-access-keys.html#iam-examples-managing-access-keys-last-used)\. 
+  For API details, see [GetAccessKeyLastUsed](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/GetAccessKeyLastUsed) in *AWS SDK for JavaScript API Reference*\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/iam/iam_basics#code-examples)\. 
  

```
def get_last_use(key_id):
    """
    Gets information about when and how a key was last used.

    :param key_id: The ID of the key to look up.
    :return: Information about the key's last use.
    """
    try:
        response = iam.meta.client.get_access_key_last_used(AccessKeyId=key_id)
        last_used_date = response['AccessKeyLastUsed'].get('LastUsedDate', None)
        last_service = response['AccessKeyLastUsed'].get('ServiceName', None)
        logger.info(
            "Key %s was last used by %s on %s to access %s.", key_id,
            response['UserName'], last_used_date, last_service)
    except ClientError:
        logger.exception("Couldn't get last use of key %s.", key_id)
        raise
    else:
        return response
```
+  For API details, see [GetAccessKeyLastUsed](https://docs.aws.amazon.com/goto/boto3/iam-2010-05-08/GetAccessKeyLastUsed) in *AWS SDK for Python \(Boto3\) API Reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, see [Using IAM with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.