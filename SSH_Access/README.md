Lab 3: SSH Configurations 

Objective
Configure secure, password-less SSH access to a remote machine using public/private key authentication and simplify connection using SSH configuration file.

1. Generate Public & Private SSH Keys

```
ssh-keygen
```

2. Securely Transfer the Public Key to the Remote Machine

```
ssh-copy-id ivolve@172.25.250.10
```

3. Configure SSH for Easy Access (ssh ivolve)

```
vim ~/.ssh/config
```
```
Host ivolve
    HostName 172.25.250.10
    User ivolve
    IdentityFile ~/.ssh/id_rsa
```

4. Test SSH Shortcut

```
ssh ivolve
```
![Alt Text](./images/ssh5.jpg)