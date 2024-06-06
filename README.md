# Managing Resources with Tagging

## Lab Overview

This lab is divided into two parts. In the Task portion, I used the AWS Command Line Interface (CLI) to inspect the tags assigned to a number of Amazon EC2 instances. I then used pre-provided scripts to shut down and start up a number of Amazon EC2 instances simultaneously, based on their tags. In the Challenge portion, I developed a solution to terminate instances that failed to implement specific tags.

![lab-7-less-instances](https://github.com/Mohamed-kittany/Canvas-Lab-188-ManagingResourcesWithTagging/assets/161580792/82c0474e-6afe-4bca-8886-f9efc4fa1378)

## Objectives

After completing this lab, I was able to:

- Apply tags to existing AWS resources.
- Find resources based on tags.
- Use the AWS CLI or AWS SDK for PHP to stop and terminate Amazon EC2 instances based on certain attributes of the resource.

## Scenario

The environment for this lab consists of:

- An Amazon VPC named Lab VPC
- A public subnet
- A private subnet
- An Amazon EC2 Linux instance named CommandHost (with AWS CLI tools pre-installed and configured)
- 8 Amazon EC2 Linux instances

Private instances have three custom tags applied to them:
- **Project**: The project that the instance belongs to (ERPSystem or Experiment1).
- **Version**: The version of the project that the instance belongs to (all set to 1.0).
- **Environment**: One of three values (development, staging, or production).

## Tasks Completed

### Task 1: Using Tags to Manage Resources

1. **Connect to the Command Host**:
   - Used SSH to connect to the CommandHost instance.
  
2. **Finding Development Instances for the Project**:
   - Used the AWS CLI to find instances tagged with `Project=ERPSystem` and `Environment=development`.
   - Applied JMESPath syntax with the AWS CLI `--query` option to return formatted output, including multiple fields like instance ID, Availability Zone, Project, Environment, and Version tags.

3. **Changing Version Tag for Development Process**:
   - Used a Bash script to change the `Version` tag for all instances marked as development for the project `ERPSystem` from 1.0 to 1.1.

### Task 2: Stop and Start Resources by Tag

1. **Examining the Stopinator Script**:
   - Reviewed the `stopinator.php` script, which uses the AWS SDK for PHP to stop and restart instances based on a set of tags.

2. **Stopping and Restarting ERPProject Development Instances**:
   - Used the `stopinator.php` script to stop and then restart all development instances for the `ERPSystem` project.

### Task 3: Challenge: Terminate Non-Compliant Instances

1. **Challenge Description**:
   - Developed a solution to automatically terminate instances in the private subnet that do not have the `Environment` tag defined.

2. **Reviewing the Tag-Or-Terminate Script**:
   - Reviewed the `terminate-instances.php` script, which uses the AWS SDK for PHP to terminate instances that do not have the `Environment` tag.

3. **Configuring Environment to Test Script**:
   - Modified the tags of certain instances to remove the `Environment` tag.

4. **Running Commands to Terminate Non-Compliant Instances**:
   - Ran the following AWS CLI commands to terminate the non-compliant instances and verify their termination status:
     ```sh
     aws ec2 terminate-instances --instance-ids i-0194b5ee52032dB56 i-0158ba240033b2afd
     aws ec2 describe-instances --instance-ids i-0194b5ee52032dB56 i-0158ba240033b2afd --query 'Reservations[*].Instances[*].{ID:InstanceId,State:State.Name}'
     ```

## Conclusion

This lab provided hands-on experience in managing AWS resources using tags. I used the AWS CLI and PHP scripts to find, modify, stop, start, and terminate EC2 instances based on their tags, ensuring efficient and automated resource management.

