# teamcity-winagent
Ansible role to provision TeamCity build agent on a Windows machine for existing TeamCity server at AWS

## Install TeamCity Windows Build Agent

### Vagrant@localhost

To provision it with your local vagrant, `cd` to project root and command: `vagrant up`

It takes 10-20 mins to get it up and running, then you can login as Administrator with password vagrant or Admin123, I don't remember.

### AWS

To provision Windows Server 2012r2 ec2 with TeamCity build agent installed on it:
```
ansible-playbook playawswinagent.yml -i ec2.py -e "teamcity_server_url=http://ec2-teamcity-ip-address.compute-1.amazonaws.com"
```

#### Known issues
- Paravirtual Virtualization type is NOT available on Windows AMIs and ec2s so we cannot use spot instances.
- Windows Server 2016 Base VM takes too long to provision.
- Windows Server 2016 Base Nano has some issues with winrm connectivity on Mac:
https://github.com/PowerShell/PowerShell/blob/master/docs/KNOWNISSUES.md#remoting-support

## Roles

- ec2-windows: Spins up ec2 Windows instance
- teamcity-winagent: Install TeamCity Windows Agent
