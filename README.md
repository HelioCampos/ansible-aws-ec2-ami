AWS ec2 AMI [![CircleCI](https://circleci.com/gh/HelioCampos/ansible-aws-ec2-ami.svg?style=svg)](https://circleci.com/gh/HelioCampos/ansible-aws-ec2-ami)
=========

This ansible create or destroy an image in ec2. 
When ec2_ami_on_build_terminate is defined, the instance will be terminated after ami was created.

Requirements
------------

- Ansible 2.0.1 or higher.
- Tested on Ubuntu 14.04 and Amazon 7

Role Variables
--------------

| parameter             | required | default | choices | comments |
| --------------------- | -------- | ------- | -------- |-------- |
| ec2_ami_instance_id| yes | | | instance id of the image to create|
| ec2_ami_on_build_terminate| no | |  | If not specified then the ec2 will not terminate after ami build|
| device_mapping | no | | | An optional list of device hashes/dictionaries with custom configurations (same block-device-mapping parameters). Valid properties include: device_name, volume_type, size (in GB), delete_on_termination (boolean), no_device (boolean), snapshot_id, iops (for io1 volume_type)|
| companys_project_build | yes | | | Ami name |
| wait_timeout | no |1000 | | how long before wait gives up, in seconds|
| aws_resource_tags  | yes  |   | | a hash/dictionary of tags to add to the new instance or for starting/stopping instance by tag; '{"key":"value"}' and '{"VREnv":"PROD","VRProject":"sample","VRTeam":"infra", "Name":"ami name"}' |
| state |  no |  present |present, absent | create or deregister/delete image  |
| region |  yes |   || The AWS region to use. Must be specified if ec2_url is not used. If not specified then the value of the EC2_REGION environment variable, if any, is used. See http://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region  |


Output variables
--------------
    ec2_ami_image_id: id of created ami 

Example Playbook
----------------

    - hosts: localhost
      vars:
        aws_resource_tags: {
         'Name': 'instance-name',
         'VREnv': 'PROD',
         'VRProject': 'infra-ansible',
         'VRTeam': 'infra',
#         'zabbix-metadata': 'if-ansible-sample'
        }
        region: us-east-1
        companys_project_name: amzlinux-docker-base-ami
        companys_build_version: 1
        companys_project_build: "{{ companys_project_name }}-{{ companys_build_version}}
        ec2_ami_on_build_terminate: true

      roles:
        - { role: aws-ec2-ami }

Ansible modules
--------------

[ec2](http://docs.ansible.com/ansible/ec2_module.html)

[ec2_ami](http://docs.ansible.com/ansible/ec2_ami_module.html)


License
-------

BSD

Original Author:
------------------

Giancarlo Rubio (<gianrubio@gmail.com>)

Modified by:
------------------

Helio Campos Mello de Andrade (<helio.campos@gmail.com>)