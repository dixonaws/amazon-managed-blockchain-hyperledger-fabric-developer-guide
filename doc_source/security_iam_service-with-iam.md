# How Hyperledger Fabric on Amazon Managed Blockchain works with IAM<a name="security_iam_service-with-iam"></a>

Before you use IAM to manage access to Hyperledger Fabric on Managed Blockchain, you should understand what IAM features are available to use with Hyperledger Fabric on Managed Blockchain\. To get a high\-level view of how Hyperledger Fabric on Managed Blockchain and other AWS services work with IAM, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

**Topics**
+ [Hyperledger Fabric on Managed Blockchain identity\-based policies](#security_iam_service-with-iam-id-based-policies)
+ [Hyperledger Fabric on Managed Blockchain Resource\-Based Policies](#security_iam_service-with-iam-resource-based-policies)
+ [Authorization based on Hyperledger Fabric on Managed Blockchain tags](#security_managed-blockchain-with-iam-tags)

## Hyperledger Fabric on Managed Blockchain identity\-based policies<a name="security_iam_service-with-iam-id-based-policies"></a>

With IAM identity\-based policies, you can specify allowed or denied actions and resources as well as the conditions under which actions are allowed or denied\. Hyperledger Fabric on Managed Blockchain supports specific actions and resources\. To learn about all of the elements that you use in a JSON policy, see [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Actions<a name="security_iam_service-with-iam-id-based-policies-actions"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Action` element of a JSON policy describes the actions that you can use to allow or deny access in a policy\. Policy actions usually have the same name as the associated AWS API operation\. There are some exceptions, such as *permission\-only actions* that don't have a matching API operation\. There are also some operations that require multiple actions in a policy\. These additional actions are called *dependent actions*\.

Include actions in a policy to grant permissions to perform the associated operation\.

Policy actions in Hyperledger Fabric on Managed Blockchain use the following prefix before the action: `managedblockchain:`\. For example, to grant someone permission to create a node with the Managed Blockchain `CreateNode` API operation, you include the `managedblockchain:CreateNode` action in their policy\. Policy statements must include either an `Action` or `NotAction` element\. Hyperledger Fabric on Managed Blockchain defines its own set of actions that describe tasks that you can perform with this service\.

To specify multiple actions in a single statement, separate them with commas as follows\.

```
"Action": [
      "managedblockchain:action1",
      "managedblockchain:action2"
```

You can specify multiple actions using wildcards \(\*\)\. For example, to specify all actions that begin with the word `List`, include the following action\.

```
"Action": "managedblockchain:List*"
```

To see a list of Managed Blockchain actions, see [Actions Defined by Amazon Managed Blockchain](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonmanagedblockchain.html#amazonmanagedblockchain-actions-as-permissions) in the *IAM User Guide*\.

### Resources<a name="security_iam_service-with-iam-id-based-policies-resources"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Resource` JSON policy element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. As a best practice, specify a resource using its [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\. You can do this for actions that support a specific resource type, known as *resource\-level permissions*\.

For actions that don't support resource\-level permissions, such as listing operations, use a wildcard \(\*\) to indicate that the statement applies to all resources\.

```
"Resource": "*"
```

Managed Blockchain resource types that can be used in IAM permissions policy statements for resources on Ethereum networks include the following:
+ network
+ member
+ node
+ proposal
+ invitation

Members, nodes, and invitations are associated with your account\. Networks and proposals, on the other hand, are scoped to the entire blockchain network and are not associated with a particular account\.

For example a network resource on Managed Blockchain has the following ARN\.

```
arn:${Partition}:managedblockchain:${Region}::networks/${NetworkId}
```

For example, to specify the `n-MWY63ZJZU5HGNCMBQER7IN6OIU` network in your statement, use the following ARN\.

```
"Resource": "arn:aws:managedblockchain:us-east-1::networks/n-MWY63ZJZU5HGNCMBQER7IN6OIU"
```

To specify any network that is visible to your account, use the wildcard \(\*\)\.

```
"Resource": "arn:aws:managedblockchain:us-east-1::networks/*"
```

Some Managed Blockchain actions, such as `CreateNetwork`, `ListInvitations`, and `ListNetworks` cannot be performed on a specific resource\. In those cases, you must use the wildcard \(\*\)\.

```
"Resource": "*"
```

To see a list of Hyperledger Fabric on Managed Blockchain resource types and their ARNs, see [Resources Defined by Amazon Managed Blockchain](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonmanagedblockchain.html#amazonmanagedblockchain-resources-for-iam-policies) in the *IAM User Guide*\. To learn with which actions you can specify the ARN of each resource, see [Actions Defined by Amazon Managed Blockchain](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonmanagedblockchain.html#amazonmanagedblockchain-actions-as-permissions)\.

### Condition keys<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>

Hyperledger Fabric on Managed Blockchain does not provide any service\-specific condition keys, but it does support using some global condition keys\. To see all AWS global condition keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

### Examples<a name="security_iam_service-with-iam-id-based-policies-examples"></a>

To view examples of Hyperledger Fabric on Managed Blockchain identity\-based policies, see [Hyperledger Fabric on Amazon Managed Blockchain Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.

## Hyperledger Fabric on Managed Blockchain Resource\-Based Policies<a name="security_iam_service-with-iam-resource-based-policies"></a>

Hyperledger Fabric on Managed Blockchain does not support resource\-based policies\.

## Authorization based on Hyperledger Fabric on Managed Blockchain tags<a name="security_managed-blockchain-with-iam-tags"></a>

You can attach tags to Hyperledger Fabric on Managed Blockchain resources or pass tags in a request to Managed Blockchain\. To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the `managedblockchain:ResourceTag/key-name`, `aws:RequestTag/key-name`, or `aws:TagKeys` condition keys\. For more information about tagging Hyperledger Fabric on Managed Blockchain resources, see [Tagging Amazon Managed Blockchain resources](tagging.md)\.

To view example identity\-based policies for allowing or denying access to resources and actions based on tags, see [Controlling access using tags](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-tags)\.