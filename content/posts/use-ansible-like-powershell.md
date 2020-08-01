---
author:
  name: "Josh Duffney"
date: 2019-08-26
linktitle: Use Ansible like PowerShell
type: post
type: posts
toc: true
title: Use Ansible like PowerShell
aliases: 
- /UsingAnsibleLikePowerShell
tags: ["PowerShell","ansible"]
categories: ["how-to"]
---

I've been using Ansible to manage a Windows environment for a little over a year now and I wanted to share a method of using Ansible that's helped me adopt it. Coming from a Windows background, I obviously have a strong PowerShell background and to be honest I struggled and still struggle sometimes using Ansible. The reason being, my default behavior is to open up a PowerShell prompt and start hacking away. 

Skilling up with Ansible has been somewhat difficult because I heavily lean on PowerShell when I need to do something quickly. What I've discovered is that by not taking those as opportunities to learn more Ansible I was slowing down my acquisition of Ansible knowledge. After realizing that I put some thought into how I could use Ansible more like I use PowerShell. Which made it easier for me to gravitate toward writing a playbook instead of writing a script. Using the method below I've started to shift my default behavior and have begun to enjoy learning Ansible a lot more.

# Target All, All the Time

Ansible has the concept of groups. Those groups are defined in a host or inventory file. As the name implies groups are used to... well group machines together. Typically by type or function. For example, webserver or database. Being a PowerShell user, I've never really had to worry about an inventory per say or groups. I either knew the host names or the host name patterns and specified them or converted them to an array to be used with `Invoke-Command` or other cmdlets. Having to maintain an inventory for my purposes here was cumbersome and for that reason I default to using the `all` group in my playbooks. Unless I know the specific host names for the playbook I just leave the hosts section of the playbook set to `all` as shown below.

```yaml
---
- hosts: all
```


# Define Connection Variables

 Because Ansible was developed to manage Linux systems first it's no surprise that it's default is to connect with ssh on port 22. That's a problem for those of us who manage Windows systems. Luckily it's a problem easily solved by defining some Ansible variables. There are A LOT of places you can define Ansible variables and this is one thing that tripped me up for awhile. Having these variables defined in a hostfile or group_vars location made Ansible seem to heavy. I wanted something light weight where I could write out a few tasks and target the systems I needed to without having to parse a hostfile or figure out which group had the correct variables. The solution to this problem is pretty simple. Just define the variables at the top of the playbook. Ansible offers several authentication options, which you can read more about on the [Windows Remote Management Authentication Options](https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html#authentication-options) wiki page. In this example, I'm using ntlm over WinRM with http. Previous to the variable `ansible_winrm_message_encryption`, you'd have to generate a self-signed certificate in order to setup a Windows host. Now, you don't have to worry about that. By specifiying `ansible_winrm_message_encryption: always` Ansible will enable message encryption and WinRM will be happy. You have a few different options for available and you can learn about about in the [Setting Up a Windows Host](https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html#setting-up-a-windows-host) wiki page. Special thanks to [Jeremy Murrah](https://twitter.com/JeremyMurrah) for pointing out the ansible_winrm_message_encryption [option to me](https://twitter.com/JeremyMurrah/status/1166783597065506821?s=20)!

```yaml
- hosts: all
  vars:
    ansible_user: Administrator
    ansible_password: P@ssw0rd
    ansible_port: 5985
    ansible_connection: winrm
    ansible_winrm_transport: ntlm
    ansible_winrm_server_cert_validation: ignore
    ansible_winrm_message_encryption: always
```

# Adding Tasks

Now the fun begins! Tasks in Ansible are what do things. Tasks leverage Ansible modules to perform actions. There's an impressive amount of Ansible modules available for Windows. For a full list check out the [Windows modules index](https://docs.ansible.com/ansible/latest/modules/list_of_windows_modules.html#windows-modules). It is with these Ansible modules the real benefit of using Ansible starts to reveal itself. Instead of having to develop and maintain your own scripts, functions, modules or even DSC resources you can now simply leverage the collective contributions made to Ansible. Each module is capable of performing idempotent actions on a target machine. The other massive benefit of using Ansible is the tooling it provides. Anyone who's used DSC quickly realizes there is a set of functionality that doesn't exist. Functionality that is needed to orchestrate the configurations and different actions required to reach your desired state. Ansible fills that gap quite nicely.

Below I've added a task to my playbook named _install dotnet core iis hosting module no frameworks_. It's leveraging the [win_chocolatey](https://docs.ansible.com/ansible/latest/modules/win_chocolatey_module.html#win-chocolatey-module) Ansible module to install the dotnet core server hosting module and installing it without frameworks. If you've used DSC before tasks will look very similar to resources in a configuration. You specify the module name (resource name) and then the properties and values required for that task.

```yaml
---
- hosts: all
  vars:
    ansible_port: 5986
    ansible_connection: winrm
    ansible_winrm_transport: kerberos
    ansible_winrm_scheme: https
    ansible_winrm_server_cert_validation: ignore

  tasks:
  - name: install dotnet core iis hosting module with no frameworks
    win_chocolatey:
      name: "microsoft-dotnet-core-windows-server-hosting"
      version: "2.2.4"
      install_args: "OPT_NO_RUNTIME=1 OPT_NO_SHAREDFX=1 OPT_NO_X86=1 OPT_NO_SHARED_CONFIG_CHECK=1"
      state: present
```

# Executing Playbooks without a Hosts File or Inventory

At this point, I have setup my Windows machines to talk to Ansible and I have written a playbook. Next, is to execute the playbook. As I previously mentioned, for this workflow I didn't want to maintain a hosts or inventory file. So, what do I do? Ansible needs that in order to target machines. If you just pass hosts names to it like this `--limit wintest01` it will throw an error complaining about not being able to parse a hosts file. Using some Google foo, I discovered this neat little snippet on GitHub [run-ansible-with-any-host-without-inventory](https://gist.github.com/lilongen/ebc11f69ae2ba48971c77527d5c02fab). TL:DR is by simply adding a `,` after the host names passed to the `-i` parameter you can bypass Ansible's default behavior of attempting to parse the value provided to the parameter.

```yaml
ansible-playbook dotnet-core-windows-server-hosting.yaml -i wintest01.domain.com,wintest02.domain.com,
```


# Am I Ansibling Wrong?

Probably, but it's a worthflow that's light enough that I'm more inclined to write a playbook than I am to write a script. 

However, this isn't the end of the road. Just like with PowerShell when you write a script and you reuse it a lot. It will eventually become a function. If that function is used alot and is then used by other people. That function will then be put in a module. My point is, it's a progression. The playbooks I created in this manner are the first progression. 

I see the logic in these playbooks becoming my referenceable artifacts that I can port into Ansible roles or custom PowerShell modules for Ansible to use. With all that said, I'm still very new to Ansible and I'm always willing to learn. Is there a better way than this? For those of you already using Ansible; what does your development workflow look like? what problems have you encountered?

---

<script async data-uid="fc6ecb784a" src="https://unique-writer-1890.ck.page/fc6ecb784a/index.js"></script>