# AAP on AWS Extension Nodes Playbooks

## About

These playbooks simplify the deployment and management of the extension nodes on AAP on AWS.

## Getting Started 

These playbooks require an active AAP on AWS foundation stack to be running. The instructions below guide how to add the credential, project, and templates required. Once added, they can be launched to add or remove the extension nodes.

1. In the AAP Controller UI, add your AWS credentials.
    1. Nav bar: Resources -> Credentials
    2. Click `Add`
    3. Enter credential name
    4. For **Credential Type** select `Amazon Web Services`
    5. Fill in AWS creds
    6. Click `Save`
2. Add this project
    1. Nav bar: Resources -> Projects
    2. Click `Add`
    3. Project Name: AWS Playbooks
    4. For  **Source Control Type** select `Git`
    5. Paste `https://github.com/zkayyali812/playbooks` in **Source Control URL**
    6. For **Source Control Branch/Tag/Commit**, enter `main` or a development branch name
    7. Click `Save`
3. Add the template - Add Extension Nodes
    1. Nav bar: Resources -> Templates
    2. Click: Add -> Add job template
    3. Name: `Add Extension Nodes`
    4. Project: `AWS Playbooks`
    5. Playbook: `extension_nodes/add_extension_nodes.yml`
    6. Credentials:
        1. Select Category - `Amazon Web Services`
        2. Select your AWS Creds
    7. Variables -
        ```
        ---
        foundation_stack_name: ""                       # Name of your foundation stack
        region: us-east-1                               # AWS region of foundation stack
        launch_template_name: extension-group-template  # EC2 Launch template name
        autoscaling_group_name: extension-group-asg     # Autoscaling group name
        instance_type: m5.large                         # Instance type (m5.large, m5.xlarge, or m5.2xlarge)
        asg_min_size: 1                                 # ASG minimum size
        asg_max_size: 1                                 # ASG maximum size
        asg_desired_capacity: 1                         # ASG desired capacity
        ```
    8. Click `Save`
4. Add the template - Remove Extension Nodes
    1. Nav bar: Resources -> Templates
    2. Click: Add -> Add job template
    3. Name: `Remove Extension Nodes`
    4. Project: `AWS Playbooks`
    5. Playbook: `extension_nodes/remove_extension_nodes.yml`
    6. Credentials:
        1. Select Category - `Amazon Web Services`
        2. Select your AWS Creds
    7. Variables -
        ```
        ---
        foundation_stack_name: ""                       # Name of your foundation stack
        region: us-east-1                               # AWS region of foundation stack
        launch_template_name: extension-group-template  # EC2 Launch template name
        autoscaling_group_name: extension-group-asg     # Autoscaling group name
        ```
    8. Click `Save`
 
## Running using Ansible Navigator

To run these playbooks locally using ansible navigator, the following commands can be used as well. Be sure to set your AWS credentials via the environment variables below before running.

### Running Add Extension Nodes playbook

```bash
ansible-navigator run playbooks/extension_nodes/add_extension_nodes.yml --set-environment-variable AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY -m stdout --playbook-artifact-enable false
```

### Running Remove Extension Nodes playbook
```bash
ansible-navigator run playbooks/extension_nodes/remove_extension_nodes.yml --set-environment-variable AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY -m stdout --playbook-artifact-enable false
```

