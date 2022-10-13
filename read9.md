## K8-Security
Configure a Security Context for a Pod or Container
AppArmor is a linux security module   restrict the capabilities of individual programs ex: deny writing to files 
Seccomp Filter a process's system calls.We can restrict syscalls(about 435 in linux) with seccomp (Linux syscalls to hardware like touch helper program is tracee)
Minimize  roles, Restrict network address 
UFW uncomplicated firewall close access to ports like ufw all of from ip to any port
allowPrivilegeEscalation: Controls whether a process can gain more privileges than its parent process. This bool directly controls whether the no_new_privs flag gets set on the container process.
allowPrivilegeEscalation is always true when the container:
is run as privileged, or
has CAP_SYS_ADMIN
readOnlyRootFilesystem: Mounts the container's root filesystem as read-only.
Discretionary Access Control: Permission to access an object, like a file, is based on user ID (UID) and group ID (GID).
Security Enhanced Linux (SELinux): Objects are assigned security labels.
Running as privileged or unprivileged.
Linux Capabilities: Give a process some privileges, but not all the privileges of the root user.
ssh hardening can not connect as root  or no password authentication only with private key

Container Sandboxing
gVisor by google is a proxy between linux-kernel  and each of the containers and the aim is isolation for security
Kata Containers: Each container has its own kernel and are lightweight but more resource and less performance . Cloud providers are already virtual machines. We use 
Vm inside vm nested virtualization.google cloud has support.Open Policy Agent is about only authorization . We use a policy language rego that is esay to read and write
