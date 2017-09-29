AWS CloudFormation template that deploys an EC2-based approach to automating snapshots of EBS volumes

## Why?

There are many solutions to this problem availble on the Internet.  Most leverage AWS Lambda, and that is a great approach.  Unfortunately, a customer I support is not yet comfortable with Lambda, so that service is not approved for use (yet).  I needed to get creative, so I developed an different approach. 

## Features

- Snapshots are performed on a fixed schedule (configurable)
- Snapshots are retained for a set number of days (configurable)
- Snapshots are automatically deleted once expired
- Auto Scaling Group spans all Availability Zones in the given AWS region to achieve high availability
- Auto Scaling Group provides self-healing capabilities to ensure a running instance at all times
- Launch Configuration fully bootstraps new instances (assumes CentOS 7 base AMI)
- CloudFormation stack provisions requisite IAM resources

## Prerequisites

- AWS account
- IAM user with sufficient privileges + access key
- Create a key pair in the desired AWS region
- Create a Security Group for your EC2 instance
- Find the ID of a trusted CentOS 7 base AMI in the target AWS region
  - The provided EC2 bootstrap script (`UserData`) assumes CentOS 7
- If you intend to use the provided helper scripts (see below), you will also need to:
  - Install the [AWS CLI](https://aws.amazon.com/cli/)
  - Run `aws configure` to set the desired region, access key, and secret access key

## Usage

The `scripts` directory contains three helper script templates:

- `validate-template.sh`
  - helper script to validate the CloudFormation template
  - useful if you make modifications to the template
  - be sure to substitute the appropriate values for the `profile` and `region` options
- `create-stack.sh`
  - helper script to create the CloudFormation stack
  - be sure to substitute the appropriate values for the `profile`, `region`, and `stack-name` options
  - be sure to pass in appropriate values for the `SshKey`, `SecurityGroupId`, `CentOS7AmiId`, and `InstanceType` parameters
- `delete-stack.sh`
  - helper script to delete the CloudFormation stack and cleanup all resources that were created