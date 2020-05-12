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

>```
> >>> conda initialize >>>
>     copy content
> <<< conda initialize <<<
>```

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
>cd ansible_folder
>```

>```
>pip install ansible boto boto3
>```

>```
>ssh-keygen -t rsa -b 4096 -f ~/.ssh/hamza_aws
>```

