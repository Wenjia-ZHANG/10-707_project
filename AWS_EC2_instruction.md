# 10-707 Advanced Deep Learning Course Project

## Set up AWS environment to train a pytorch model

1. Open AWS Console
2. Find and open EC2 Dashboard
3. Choose "Launch instance"
4. Choose AMI (virtual machine image on AWS)
   1. Seach for `AWS Deep Learning AMI (Ubuntu 18.04)` and select -> continue
5. Choose instance: `g4dn.xlarge` and click "Next: Configure Instance Details"
6. Don't need to change anything, click next
7. Storage: don't change anything, click next
8. Tag: no need to add tag, click next
9. Configure security group: need to change the group to allow ssh and http
   1.  Allow SSH and HTTP type in the security group
   2.  Click review and launch
10. Click "Launch" and need to create (or select) an SSH key pair, need to store the generated private key in your laptop and remember the location
11. Launch the instance and wait for 5 mins until it's fully set up
12. Go to "EC2 Dashboard" then "Instances (running)" and click your "Instance ID" to see the details of it
13. Use VSCode to ssh to the instance
    1.  Open VSCode and install SSH remote extension
    2.  Open ssh config file and add a new entry as below:
    3.  Connect to host and choose "aws-gpu"
    4.  In VSCode, "Terminal" -> "New Terminal", it will open a terminal on the aws virtual machine

```bash
Host aws-gpu
  HostName <"Public IPv4 DNS" on instance summary page>
  User ubuntu
  IdentityFile "/path/to/your/private/key/file.pem"
```

14. Try to run a python code with conda
    1.  In terminal, check all environments: `conda env list`
    2.  Activate pytorch environment with conda: `conda activate pytorch_latest_p37`
    3.  Check python and pytorch version: `python --version`

```
(pytorch_latest_p37) ubuntu@ip-172-31-94-177:~$ python --version
Python 3.7.10
(pytorch_latest_p37) ubuntu@ip-172-31-94-177:~$ python
Python 3.7.10 | packaged by conda-forge | (default, Feb 19 2021, 16:07:37) 
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> torch.__version__
'1.8.1+cu111'
>>> x = torch.rand(5, 3)
>>> print(x)
tensor([[0.6290, 0.5021, 0.4742],
        [0.3149, 0.3215, 0.8427],
        [0.0952, 0.9669, 0.6490],
        [0.9362, 0.0779, 0.2307],
        [0.5043, 0.2518, 0.8508]])
>>> torch.cuda.is_available()
True
```

15. Add SSH key of AWS instance to Github
    1. Generate ssh key on aws instance: `ssh-keygen -t rsa -b 4096 -C "${user}@${example}.com"`
    2. Add `~/.ssh/id_rsa.pub` to GitHub -> Setting -> SSH and GPG keys -> New SSH key
    3. Append the two lines to `~/.bashrc` file: `eval $(ssh-agent -s)` and `ssh-add ~/.ssh/id_rsa`
    4. `source ~/.bashrc`
    5. `git config --global user.email "${user}@${example}.com"`
    6. `git config --global user.name "${username}"`
    7. Clone the repo: `git clone <repo ssh link>`

### Links

- AWS GPU instances: https://docs.aws.amazon.com/dlami/latest/devguide/gpu.html
- G4 (Nvidia T4) might be good: https://aws.amazon.com/ec2/instance-types/g4/
  - It seems to me that `g4dn.xlarge` or `g4dn.2xlarge` might be good for the project purpose.
- AWS AMI Choice
  - It looks like this one is good for our purpose: https://aws.amazon.com/marketplace/pp/prodview-x5nivojpquy6y
  - It has conda, pytorch, and Nvidia GPU drivers
