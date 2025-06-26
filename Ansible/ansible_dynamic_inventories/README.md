# Lab 8: Ansible Dynamic Inventories🔑💻

## Overview 
Set up a dynamic inventory in Ansible that automatically discovers AWS EC2 instances with the tag name=ivolve, and verify using the ansible-inventory command.

## ✅ Prerequisites

🧑‍💻 AWS account

🔑 AWS CLI configured (aws configure)

🗝️ EC2 key pair & security group allowing SSH (port 22)

🐍 Python 3.x installed

📦 Required Python & Ansible packages:

```
pip install boto3 botocore ansible
```
🔐 Make sure your SSH key is secure:

```
chmod 400 ~/path/to/ansible.pem
```

## ✅ Project Structure

```
ansible-ec2/
├── ansible.cfg       
├── aws_ec2.yaml      
└── ping.yml      
```

## 🚀 Step 1: Launch EC2 Instance with Tag name=ivolve

```
aws ec2 run-instances \
    --image-id ami-0c02fb55956c7d316 \  
    --count 1 \
    --instance-type t2.micro \
    --key-name ansible \
    --tag-specifications 'ResourceType=instance,Tags=[{Key=name,Value=ivolve}]'
```

## 📁 Step 2: Create Inventory Configuration File


📝 Create ansible.cfg:

```
[defaults]
inventory = aws_ec2.yaml
host_key_checking = False
timeout = 30
remote_user = ec2-user
private_key_file = /root/ansible.pem
```

📝 Create aws_ec2.yml:

```
plugin: amazon.aws.aws_ec2
regions:
  - us-east-1
filters:
  tag:name: ivolve
keyed_groups:
  - key: tags.name
    prefix: ec2_tag
hostnames:
  - private-ip-address
compose:
  ansible_host: public_ip_address
```

## 🔐 Step 3: Set AWS Credentials

```
aws configure
```

## 📡 Step 4: List Hosts in Dynamic Inventory

```
ansible-inventory --graph
```
```
@all:
  |--@ungrouped:
  |--@aws_ec2:
  |  |--172.31.87.185
  |--@ec2_tag_ivolve:
  |  |--172.31.87.185
```

## 📁 Step 5: Create Playbook File

```
---
- name: Ping EC2 instance with tag Name=ivolve
  hosts: ec2_tag_ivolve
  gather_facts: no
  tasks:
    - name: Ping the instance
      ping:   
```

## 🧪 Step 6: Test Ansible Ping

```
ansible -m ping ec2_tag_ivolve   -u ec2-user   --private-key ~/ansible.pem
```
output:

```
72.31.87.185 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.9"
    },
    "changed": false,
    "ping": "pong"
}
```

## ✅ Step 7: Run the Playbook

```
ansible-playbook ping.yml
```
Output:

```
ok: [172.31.87.185]

PLAY RECAP *********************************************************************
172.31.87.185              : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

```

```
.
├── build
│   ├── classes
│   │   └── java
│   │       ├── main
│   │       │   └── com
│   │       │       └── ivolve
│   │       │           └── App.class
│   │       └── test
│   │           └── com
│   │               └── ivolve
│   │                   └── AppTest.class
│   ├── distributions
│   │   ├── build1-1.0.tar
│   │   └── build1-1.0.zip
│   ├── generated
│   │   └── sources
│   │       ├── annotationProcessor
│   │       │   └── java
│   │       │       ├── main
│   │       │       └── test
│   │       └── headers
│   │           └── java
│   │               ├── main
│   │               └── test
│   ├── libs
│   │   └── ivolve-app.jar
│   ├── reports
│   │   ├── problems
│   │   │   └── problems-report.html
│   │   └── tests
│   │       └── test
│   │           ├── classes
│   │           │   └── com.ivolve.AppTest.html
│   │           ├── css
│   │           │   ├── base-style.css
│   │           │   └── style.css
│   │           ├── index.html
│   │           ├── js
│   │           │   └── report.js
│   │           └── packages
│   │               └── com.ivolve.html
│   ├── scripts
│   │   ├── build1
│   │   └── build1.bat
│   ├── test-results
│   │   └── test
│   │       ├── binary
│   │       │   ├── output.bin
│   │       │   ├── output.bin.idx
│   │       │   └── results.bin
│   │       └── TEST-com.ivolve.AppTest.xml
│   └── tmp
│       ├── compileJava
│       │   ├── compileTransaction
│       │   │   ├── backup-dir
│       │   │   └── stash-dir
│       │   │       └── App.class.uniqueId0
│       │   └── previous-compilation-data.bin
│       ├── compileTestJava
│       │   └── previous-compilation-data.bin
│       ├── jar
│       │   └── MANIFEST.MF
│       └── test
├── build.gradle
├── settings.gradle
└── src
    ├── main
    │   └── java
    │       └── com
    │           └── ivolve
    │               └── App.java
    └── test
        └── java
            └── com
                └── ivolve
                    └── AppTest.java

51 directories, 26 files

```




