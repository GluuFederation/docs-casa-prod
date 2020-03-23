# Gluu Casa Quick Start Guide

## Overview

Gluu Casa is a self-service web portal for end-users to manage security preferences for their account in a [Gluu Server](https://gluu.org). Follow this guide to install and configure a deployment of Casa.

## Prerequisites

Casa can be installed on a VM with a pre-existing Gluu Server installation, or a new instance of the Gluu Server can be installed during Casa deployment. 

The server or VM should have at least the following minimum specs:

| CPU Unit | RAM | Disk Space | Processor Type |
|------ | ---- | ---- | ---- |
| 2  | 5 GB | 40 GB | 64 Bit |

The following operating systems are supported:

- Ubuntu 16/18
- CentOS 7
- RHEL 7
- Debian 9

## Getting started

### Install packages

Casa requires an operational Gluu Server with a matching version number (e.g. Casa 4.0 and Gluu 4.0). In addition, Casa should be installed on the same server or virtual machine as the Gluu Server. 

- Follow the [Casa installation instructions](./installation/installation.md#installation-via-linux-packages) to install the packages.  

If you don't already have the Gluu Server installed, it will be automatically installed with Casa.
    
*Do **not** run setup_casa.py at this time.*

### Set up Gluu Server

If the Gluu Server was installed in the previous step, follow these instructions to [start the server](https://gluu.org/docs/ce/4.0/installation-guide/install/#start-the-server-and-log-in) and [run the setup script](https://gluu.org/docs/ce/4.0/installation-guide/install/#run-setuppy). 

Otherwise, skip this step.

Make sure the Gluu Server has at least the following components:

  - oxAuth OAuth2 Server
  - oxTrust admin GUI
  - Gluu LDAP 
  - Apache Web Server

### Set up Casa

Once the Gluu Server is installed, [run the Casa setup script](./installation.md#run-the-setup-script). 

The [oxd client software](https://gluu.org/docs/oxd) is a required component to integrate Casa with the Gluu Server. It will be automatically installed with the script. When prompted, select `no` to install oxd.

### Configure Casa

Configuring Casa for usage requires you to enable interception scripts in the Gluu Server, activate the authentication methods in Casa, and install and configure the 2FA settings plugin. 

1. **Interception Scripts in Gluu**: Enable authentication interception scripts in the Gluu Server. Log in to oxTrust as an administrator and [enable the desired 2FA credentials](./administration/admin-console.md#enabled-methods) to be managed with Casa.

1. **Activate authentication methods in Casa**: Once the interception scripts have been enabled, they can be activated in Casa itself. Log in to Casa as an administrator and [enable the desired methods](./administration/admin-console.md#configure-casa).

1. **Setup 2FA preferences**: Use the [2FA Settings plugin](./plugins/2fa-settings.md) to set the [minimum number of credentials](../administration/admin-console.md#2fa-settings) a user must enroll.

### Test enrollment and 2FA

1. [Enroll](./user-guide.md#2fa-credential-details-enrollment) at least two credentials on a non-administrator user.

1. [Turn on](./user-guide.md#turn-2fa-onoff) 2FA for the account.

1. Test 2FA Authentication by logging off and logging back in. Application access should now require a second authentication factor.

### Finish configuration

Once satisfied with testing, [configure the Gluu Server](./administration/admin-console.md/#set-default-authentication-method-gluu) to log in users via Casa for all applications the server protects.

### Check out available plugins

Browse our [catalog of plugins](https://casa.gluu.org/plugins) to add features and expand Casa!
