### What is Ansible?
```bash
- Ansible is an opensource IT configuration management tool. It enables DevOps teams to define their infrastructure as a code in a
  simple and way.
- Most of peoples compare Ansible to similar tool like Puppet and Chef, they all use for automate and provision infrastructure,
  but due to some features Ansible are more preferable as compare to other.
- Ansible can also be considered for administrators and operations systems to ensure control over multiple servers.
```
### How Ansible Works?
![image](https://github.com/user-attachments/assets/43a421ef-859d-44df-a6a0-301ba7301838)

### Benefits of Ansible:
Benefits Of Ansible
	Simple – It is very simple to use and is supported by YAML.
	Agentless – There are no agents or software deployed on the clients/servers to work with Ansible. The connection can be done through the SSH or using the Python.
	Efficient – There are no servers, daemons, or databases required for Ansible to work.
	Performance- The Ansible’s performance is excellent and has very little latency.
	Secure and consistent – Since the Ansible uses SSH and Python it is very secure, and the operations are flawless.
	Reliable – The Ansible Playbook can be used to write programs or the modules and can be used to manage the IT without any downside.


###  1. What is Gathering facts?
```bash
In Ansible, gathering facts means collecting system information (a.k.a. "facts") about the remote host(s) before running any tasks.
These facts include things like:

- OS type and version
- IP address and hostname
- Available memory and CPU info
- Network interfaces
- Disk space
- Environment variables
- Default shell, Python version, etc.


```
