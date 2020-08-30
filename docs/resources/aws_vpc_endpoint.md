---
title: About the aws_vpc_endpoint Resource
platform: aws
---

# aws\_vpc\_endpoint

Use the `aws_vpc_endpoint` InSpec audit resource to test properties of a single specific AWS VPC Endpoint. 

A VPC Endpoint is uniquely identified by the VPC Endpoint ID (e.g vpce-123456abcdef12345)

## Syntax

Ensure that a VPC Endpoint exists.

    # Find a VPC Endpoint by ID
    describe aws_vpc_endpoint('vpce-12345678987654321') do
      it { should exist }
    end
    
    # Hash syntax for ID
    describe aws_vpc_endpoint(vpc_endpoint_id: 'vpce-12345678987654321') do
      it { should exist }
    end


## Parameters

This resource expects the VPC Endpoint ID as a parameter.

See also the [AWS documentation on VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html).

## Properties

|Property           | Description|
| ---               | --- |
|vpc_endpoint_id       | The ID of the endpoint |
|vpc_endpoint_type | One of "Interface", "Gateway" |
|vpc_id           | The ID of the VPC in which the endpoint resides. |
|state | State of the VPC Endpoint. One of "PendingAcceptance", "Pending", "Available", "Deleting", "Deleted", "Rejected", "Failed", "Expired" |
|route_table_ids | The route table IDs for the Gateway type endpoint. |
|subnet_ids | The subnet IDs for the Interface type endpoint. |
|groups | The Security Groups for the Interface type endpoint. |
| private_dns_enabled   | Boolean value for Private DNS enable status.                 |
|network_interface_ids            | The Network Interface IDs for the Interface type endpoint. |
|dns_entries | The DNS Entries for the VPC Endpoint. |
|tags | The key/value combination of a tag assigned to the resource. |

## Examples

##### Ensure a VPC Endpoint is available.
    describe aws_vpc_endpoint(vpc_endpoint_id: 'vpce-12345678987654321') do
      its('state') { should eq 'Available' }
    end

##### Ensure that the endpoint is of Gateway type.
    describe aws_vpc_endpoint(vpc_endpoint_id: 'vpce-12345678987654321') do
        its('vpc_endpoint_type') { should eq 'Gateway' }
    end

##### Check tags    
    describe aws_vpc_endpoint do
      its('tags') { should include(:Environment => 'env-name',
                                   :Name => 'vpce-name')}
    end

## Matchers

This InSpec audit resource has the following special matchers. For a full list of available matchers, please visit our [Universal Matchers page](https://www.inspec.io/docs/reference/matchers/).

#### exist

The control will pass if the describe returns at least one result.

Use `should_not` to test the entity should not exist.

    describe aws_vpc_endpoint(vpc_endpoint_id: 'vpce-12345678987654321') do
      it { should exist }
    end
      
    describe aws_vpc_endpoint(vpc_endpoint_id: 'vpce-12345678987654321') do
      it { should_not exist }
    end

## AWS Permissions

Your [Principal](https://docs.aws.amazon.com/IAM/latest/UserGuide/intro-structure.html#intro-structure-principal) will need the `ec2:DescribeVpcEndpoints` action with Effect set to Allow.

You can find detailed documentation at [Actions, Resources, and Condition Keys for Amazon EC2](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonec2.html).