# Ansible AWS
 Using ansible to create EC2 instances

>```
>mkdir tmp
>```

>```
>cd tmp
>```

>```
>curl -O https://repo.anaconda.com/archive/Anaconda3-2020.02-Linux-x86_64.sh
>```

>```
>ls
>```

>```
>bash ./Anaconda3-2020.02-Linux-x86_64.sh
>```

>```
>yes
>```

>```
>Enter
>```

>```
>yes
>```

>```
>cat ~/.bashrc
>```

>```
>nano ~/.bash_profile
>```
>
>>```
>># >>> conda initialize >>>
>># !! Contents within this block are managed by 'conda init' !!
>>__conda_setup="$('/home/ubuntu/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
>>if [ $? -eq 0 ]; then
>>    eval "$__conda_setup"
>>else
>>    if [ -f "/home/ubuntu/anaconda3/etc/profile.d/conda.sh" ]; then
>>        . "/home/ubuntu/anaconda3/etc/profile.d/conda.sh"
>>    else
>>        export PATH="/home/ubuntu/anaconda3/bin:$PATH"
>>    fi
>>fi
>>unset __conda_setup
>># <<< conda initialize <<<
>>```

>```
>source ~/.bash_profile
>```

>```
>python --version
>```

>```
>conda env list
>```

>```
>conda create -n py3ans python=3.6
>```

>```
>y
>```

>```
>conda activate py3ans
>```

>```
>python --version
>```

>```
>pip --version
>```

>```
>pip install ansible
>```

>```
>ansible --version
>```

>```
>cd ;
>```

>```
>ls
>```

>```
>mkdir ansible_folder
>```

>```
>cd ansible_folder
>```

>```
>pip install ansible boto boto3
>```

>```
>ssh-keygen -t rsa -b 4096 -f ~/.ssh/hamza_aws
>```

>```
>mkdir -p aws-ansible/group_vars/all/
>```

>```
>cd aws-ansible
>```

>```
>touch playbook.yml
>```

>```
>ansible-vault create group_vars/all/pass.yml
>```

>```
>ansible-vault edit group_vars/all/pass.yml
>```

>```
>nano playbook.yml
>```
>
>>```
>># AWS playbook
>>---
>>
>>- hosts: localhost
>>   connection: local
>>   gather_facts: False
>>
>>  vars:
>>    key_name: hamza_aws
>>    region: us-east-1
>>    image: ami-085925f297f89fce1 # https://cloud-images.ubuntu.com/locator/ec2/
>>    id: "web-app"
>>    sec_group: "{{ id }}-sec"
>>
>>  tasks:
>>
>>    - name: Facts
>>      block:
>>
>>      - name: Get instances facts
>>        ec2_instance_facts:
>>          aws_access_key: "{{ec2_access_key}}"
>>          aws_secret_key: "{{ec2_secret_key}}"
>>          region: "{{ region }}"
>>        register: result
>>
>>      - name: Instances ID
>>        debug:
>>          msg: "ID: {{ item.instance_id }} - State: {{ item.state.name }} - Public DNS: {{ item.public_dns_name }}"
>>        loop: "{{ result.instances }}"
>>
>>      tags: always
>>
>>
>>    - name: Provisioning EC2 instances
>>      block:
>>
>>      - name: Upload public key to AWS
>>        ec2_key:
>>          name: "{{ key_name }}"
>>          key_material: "{{ lookup('file', '/home/ubuntu/.ssh/{{ key_name }}.pub') }}"
>>          region: "{{ region }}"
>>          aws_access_key: "{{ec2_access_key}}"
>>          aws_secret_key: "{{ec2_secret_key}}"
>>
>>      - name: Create security group
>>        ec2_group:
>>          name: "{{ sec_group }}"
>>          description: "Sec group for app {{ id }}"
>>          # vpc_id: 12345
>>          region: "{{ region }}"
>>          aws_access_key: "{{ec2_access_key}}"
>>          aws_secret_key: "{{ec2_secret_key}}"
>>          rules:
>>            - proto: tcp
>>              ports:
>>                - 22
>>              cidr_ip: 0.0.0.0/0
>>              rule_desc: allow all on ssh port
>>        register: result_sec_group
>>
>>      - name: Provision instance(s)
>>        ec2:
>>          aws_access_key: "{{ec2_access_key}}"
>>          aws_secret_key: "{{ec2_secret_key}}"
>>          key_name: "{{ key_name }}"
>>          id: "{{ id }}"
>>          group_id: "{{ result_sec_group.group_id }}"
>>          image: "{{ image }}"
>>          instance_type: t2.micro
>>          region: "{{ region }}"
>>          wait: true
>>          exact_count: 3
>>          count_tag:
>>            Name: Hamza_Ansible
>>          instance_tags:
>>            Name: Hamza_Ansible
>>
>>      tags: ['never', 'create_ec2']
>>```

>```
>ansible-playbook playbook.yml --ask-vault-pass
>```

>```
>ansible-playbook playbook.yml --ask-vault-pass --tags create_ec2
>```

>```
>ssh -i ~/.ssh/hamza_aws ubuntu@<ip-address>
>```