# Installation 
Thanks for your interest in Casa! Follow the instructions below to spin up an instance of Casa to offer end-users self-service 2FA and more for their account(s) in your Gluu Server.

View screenshots in the [User Guide](../user-guide.md).

## Gluu Server pre-requirements

Casa must be installed on the same server or VM as an operational Gluu Server 4.0 instance with at least the following components:  

- Apache     
- Backend database (LDAP / Couchbase / [Hybrid](https://gluu.org/docs/cb/4.0/#hybrid-backend) mode)  
- oxAuth    

!!! Warning
    It is required your installation was configured to use a FQDN for hostname, not an IP address

**[Install Gluu 4.0](https://gluu.org/docs/ce/4.0/installation-guide/)**

In addition, make sure your instance meets the following requirements: 

- At least **1GB of additional RAM** on the server or VM in addition to [Gluu Server system requirements](https://gluu.org/docs/ce/4.0/installation-guide/#system-requirements) 

- Enable Dynamic Client Registration: in your Gluu Server admin UI ("oxTrust"), navigate to `Configuration` > `JSON Configuration` > `oxAuth configuration`, find the `dynamicRegistrationEnabled` property, and confirm it is set to `true`.

    !!! Note  
        Dynamic client registration can be turned off after Casa installation, as needed. 
 
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

!!! Note 
    To change the default URL path for Casa follow the steps listed [here](change-context-path.md). It is advisable to apply this customization **before** credentials are enrolled. 

### Unlocking admin features

Recall admin capabilities are disabled by default. To unlock admin features follow these steps:

1. Navigate inside chroot to `/opt/gluu/jetty/casa/`
1. Create an empty file named `.administrable` (ie. `touch .administrable`)
1. [Restart](https://gluu.org/docs/ce/4.0/operation/services/#restart) casa
1. Wait a couple of minutes, then visit the URL and authenticate against Gluu to access Casa

### A word on security

In a clustered or containerized deployment, admin features and user features should run on different nodes. It is responsibility of the administrator to enable admin features on a specific (small) set of nodes and make those publically inaccessible, for instance, by removing them from the load balancer.

    
## Licensing

A casa installation has a 30 day trial period after which you need a license file for casa to work properly. Check this [page](licensing.md) for more details.
