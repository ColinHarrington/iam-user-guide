# Amazon RDS: Allows full RDS database access within a specific Region<a name="reference_policies_examples_rds_region"></a>

This example shows how you might create an identity\-based policy that allows full RDS database access within a specific Region\. This policy grants the permissions necessary to complete this action programmatically from the AWS API or AWS CLI\. To use this policy, replace the *italicized placeholder text* in the example policy with your own information\. Then, follow the directions in [create a policy](access_policies_create.md) or [edit a policy](access_policies_manage-edit.md)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "rds:*",
            "Resource": ["arn:aws:rds:region:*:*"]
        },
        {
            "Effect": "Allow",
            "Action": ["rds:Describe*"],
            "Resource": ["*"]
        }
    ]
}
```