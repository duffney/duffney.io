---
author:
  name: "Josh Duffney"
date: 2018-08-09
linktitle: Secure Group Variables with Ansible Vault
type: post
type: posts
toc: true
title: Secure Group Variables with Ansible Vault
aliases: 
- /SecureGroupVarsWithAnsibleVault
tags: ["secrets","ansible"]
categories: ["how-to"]
---

Ansible is commonly used to connect to other systems and to connect to those other systems you'll have to authenticate. However, you don't want to store your passwords in your ansible playbooks or group_vars files. This blog post covers how to use ansible valut to encrypt the values of variables stored under the group_vars directory in your ansible repos. I'll start by showing an example of a clear text password in my ansible repo and walk through the process of encrypting it with ansible valut.

# Clear Text Password Example

In the image below is my current ansible repo layout. I have a group_vars folder with one file in it called win.yml. I also have a roles directory and a single role defined called common with a main.yml file in the tasks folder. Beyond that I have one inventory file and one playbook. The only file that matters at the moment is the win.yml. This stores all the variables for my win group. Win is short for Windows if you're wondering. Inside this file are a few variables that allow ansible to connect to remote windows clients via winrm. It defines the user name with ansible_user and the password for the winrm user with ansible_password. It also defines the winrm port as 5985 and the connection to be winrm. However, the problem with this file is my ansible_password is clear text. That's what we'll be working to change.

![unsecureVarValue](/img/posts/secure-group-variables-with-ansible-vault/unsecureVarValue.png "unsecureVarValue")

If we run the ansible command with the debug module we can see the variables available to the remote systems. To do that I'll run the ansible command with the debug module and the argument `var=hostvars[inventory_hostname]` to display all the variables. Since I'm not using the default ansible location I also have to specify my inventory file and group of hosts I'd like to target. As you can see in the output below the password is visible here as well in clear text. We could solve this problem two ways. We could encrypt the entire file with ansible-value or we can separate sensitive and non-sensitive variables. I'll opt to separate the variables, encrypting the entire is a bit overkill in this example.

```powershell
ansible -m debug -a 'var=hostvars[inventory_hostname]' -i /vagrant/inventory.yml win
```

![ansibleDebugOutput](/img/posts/secure-group-variables-with-ansible-vault/ansibleDebugOutput.png "unsecuansibleDebugOutputreVarValue")

# Putting Sensitive Variables into Ansible Vault

## Create Directory Structure

Before we can encrypt the sensitive variable value we have to renme and move the files in group_vars around a bit. First we'll rename web.yml to vars. Then we'll create a directory under group_vars called win and move vars inside that new folder.

```powershell
mv group_vars/win.yml group_vars/vars
mkdir group_vars/win
mv group_vars/vars group_vars/win/
```

## Remove Sensitive Variables

Next we will remove the sensitive password from the vars file. Open the vile with nano or vi and remove the ansible_password line.

```powershell
vi group_vars/win/vars
```

The file should now look like this

```powershell
ansible_user: vagrant
ansible_port: 5985
ansible_connection: winrm
```

## Create Vault-Encrypted File for Secure Values

Next, we'll create a vault-encrypted file within the win directory to store all of our encrypted values. To do that use the ansible-vault create command. Name the file vault.

```powershell
ansible-vault create group_vars/win/vault
```

When prompted enter your desired password and after you confirm the password a new file will open. In the file, put in the sensitive variable from the previous vars folder. A common practice is to use the same variable name but prefix it with `vault_` to indicate these are defined in a vault protected file. The three `---` indicate a yaml file. Save and close the file after making the necessary changes.

```
---
vault_ansible_password: vagrant
```

The directory structure should now look like this. To compare use the three command from the root level of the ansible repo. Pay attention to the folders under the group_vars folder. You should have the win folder and two files under it one called vars and another call vault.


![tree](/img/posts/secure-group-variables-with-ansible-vault/tree.png "tree")

## Referencing Vault Variables from Unencrypted Variables

We could use the variables as they are right now with all the unsecure variables being in the vars file and all the secure variables being in the vault file. However, this does make it difficult to keep track of the variables. That's why I'll be referring the variables in the vault file inside the vars file. To do that open the vars file and make the following changes. It's not necessary to add a comment section separated the non-sensitive and sensitive variables, but it does make it clear which vars are coming from the vault file.

```powershell
vi group_vars/win/vars
```

```shell
---
# nonsensitive data
ansible_user: vagrant
ansible_port: 5985
ansible_connection: winrm

# sensitive data
ansible_password: "{{ vault_ansible_password }}"
```


## Verify the Password is Encrypted

At this point, we're done. To verify the password is no longer shown in clear text we can run the ansible debug command again.

```powershell
ansible -m debug -a 'var=hostvars[inventory_hostname]' -i /vagrant/inventory.yml win
```

Notice that when you run this command now it fails. This is because it uses encrypted variables and you now have to provide the vault password with the command. If we run it again with the --ask-vault-pass parameter and specify the password, we'll get what we would expect. 

![secureVar](/img/posts/secure-group-variables-with-ansible-vault/secureVar.png "secureVar")

As you can see the value for ansible_password is now the secure variable in vault not the clear text password.

# Conclusion

Variables can be sensitive and non-sensitive, but having them in different places reduces visibility. Putting sensitive values in a vault protected file is a must, but you can also include them in normal variable files to improve visibility.

## Sources

[How To Use Vault to Protect Sensitive Ansible Data](https://www.digitalocean.com/community/tutorials/how-to-use-vault-to-protect-sensitive-ansible-data-on-ubuntu-16-04)

<br>

---

<br>

<div align="center">
<a href="https://share.mailbrew.com/joshduffney/josh-duffney-monthly-digest-EJ7KkacwQyPl">Subscribe</a>

<br>
<br>
