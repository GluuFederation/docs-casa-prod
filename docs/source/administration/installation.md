# Installation 
Thanks for your interest in Casa! Follow the instructions below to spin up an instance of Casa to offer end-users self-service 2FA and more for their account(s) in your Gluu Server.

View screenshots in the [User Guide](../user-guide.md).

## Gluu Server pre-requirements

Casa must be installed on the same server or VM as an operational Gluu Server 4.0 instance with at least the following components:  

- Apache     
- LDAP    
- oxAuth   
- oxTrust     

**[Install Gluu 4.0](https://gluu.org/docs/ce/4.0/installation-guide/)**

In addition, make sure your instance meets the following requirements: 

- At least **1GB of additional RAM** on the server or VM in addition to [Gluu Server system requirements](https://gluu.org/docs/ce/4.0/installation-guide/#system-requirements) 

- Enable Dynamic Client Registration: in your Gluu Server admin UI ("oxTrust"), navigate to `Configuration` > `JSON Configuration` > `oxAuth configuration`, find the `dynamicRegistrationEnabled` property, and confirm it is set to `true`.

    !!! Note  
        Dynamic client registration can be turned off after Casa installation, as needed. 

- If your Gluu Server 4.0 was upgraded from 2.4.4, ensure the `uma_protection` scope is allowed for dynamic registration in oxTrust.
    
## Installation via Linux Packages 


!!! Warning 
    If you have logged into the Gluu chroot in any console terminal, log out before proceeding.

### Ubuntu 18 (bionic)

|  Command Description    |               Xenial Commands         |
|-------------------------|---------------------------------------|
| Add Gluu Repository     | `# echo "deb https://repo.gluu.org/ubuntu/ bionic main" > /etc/apt/sources.list.d/gluu-repo.list` |
| Add Gluu GPG Key        | `# curl https://repo.gluu.org/ubuntu/gluu-apt.key | apt-key add -` |
| Update/Clean Repo       | `# apt-get update`                         |
| Install Gluu Casa     | `# apt-get install gluu-casa`      |

### Ubuntu 16 (xenial)
      
|  Command Description    |               Xenial Commands         |
|-------------------------|---------------------------------------|
| Add Gluu Repository     | `# echo "deb https://repo.gluu.org/ubuntu/ xenial main" > /etc/apt/sources.list.d/gluu-repo.list` |
| Add Gluu GPG Key        | `# curl https://repo.gluu.org/ubuntu/gluu-apt.key | apt-key add -` |
| Update/Clean Repo       | `# apt-get update`                         |
| Install Gluu Casa     | `# apt-get install gluu-casa`      |

### CentOS 7
     
| Command Description     |               CentOS 7.2              |
|-------------------------|---------------------------------------|
| Add Gluu Repository     | `# wget https://repo.gluu.org/centos/Gluu-centos7.repo -O /etc/yum.repos.d/Gluu.repo` |
| Add Gluu GPG Key        | `# wget https://repo.gluu.org/centos/RPM-GPG-KEY-GLUU -O /etc/pki/rpm-gpg/RPM-GPG-KEY-GLUU`|
| Import GPG Key          | `# rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-GLUU` |
| Update/Clean Repo       | `# yum clean all`                          |
| Install Gluu Casa     | `# yum install gluu-casa`          |

### RHEL 7
     
| Command Description     |               RHEL 7                  |
|-------------------------|---------------------------------------|
| Add Gluu Repository     | `# wget https://repo.gluu.org/rhel/Gluu-rhel7.repo -O /etc/yum.repos.d/Gluu.repo` |
| Add Gluu GPG Key        | `# wget https://repo.gluu.org/rhel/RPM-GPG-KEY-GLUU -O /etc/pki/rpm-gpg/RPM-GPG-KEY-GLUU`|
| Import GPG Key          | `# rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-GLUU` |
| Update/Clean Repo       | `# yum clean all`                          |
| Install Gluu Casa     | `# yum install gluu-casa`          |

### Debian 9 (Stretch)

| Command Description     |               Jessie Commands         |
|-------------------------|---------------------------------------|
| Add Gluu Repository     | `# echo "deb https://repo.gluu.org/debian/ stable main" > /etc/apt/sources.list.d/gluu-repo.list`|
| Add Gluu GPG Key        | `# curl https://repo.gluu.org/debian/gluu-apt.key | apt-key add -` |
| Update/Clean Repo       | `# apt-get update`                         |
| Install Gluu Casa     | `# apt-get install gluu-casa`      |

    
## Run the Setup Script

The Casa setup script, `setup_casa.py`, adds the application to the Gluu Server, imports required data to LDAP, and applies a number of required configurations in the Gluu Server chroot.

Log in to the Gluu Server chroot, as follows:

`$ service gluu-server login`

(Or `gluu-serverd start` for systemd-based distros). 

Then `cd` to the setup scripts directory and run `setup_casa.py`: 

```
# cd /install/community-edition-setup
# ./setup_casa.py
```

Answer the setup questions as prompted. Hit Enter to accept the default value specified in square brackets, if appropriate. 

## Finish setup
After answering the setup questions, your selections will be displayed with a prompt to finish installation. If everything looks good, hit Y to finish.

Upon successful installation, a confirmation message will appear that says: "Casa installation successful! Point your browser to `https://<host>/casa`".

Recall admin capabilities are disabled by default.
Check casa docs to learn how to unlock admin features

 To unlock admin features in casa, Follow these steps:
1.	Navigate inside chroot to /opt/gluu/jetty/casa/  

 2.	Create a blank /empty file called .administrable <br />
`touch .administrable`<br />
(This blank file serves as a marker file to indicate that this node is designated for admin functionalities)
In a clustered deployment, the admin features and user features should run on different nodes to stay protected from a privilege escalation attack. It will be the responsibility of the administrator to enable admin features on a particular node, make it publically inaccessible and remove it from the load balancer (while still leveraging syncing of LDAP and file system by Cluster Manager).

 3.	Restart casa. <br />
`service casa restart`<br />
Wait a couple of minutes, then visit the URL and authenticate against Gluu to access Casa. 

!!! Note 
    To change the default URL path for Casa follow the steps listed [here](change-context-path.md). It is advisable to apply this customization **before** credentials are enrolled. 
