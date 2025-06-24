# Lab 3: SSH Configurations 🔐
This lab demonstrates how to set up secure, password-less SSH access to a remote machine using SSH key-based authentication, and how to simplify repeated connections using an SSH configuration file.

## 🎯 Objective

Generate a public/private SSH key pair
Enable password-less authentication by copying the public key to a remote machine
Configure a shortcut in the SSH config file for easier access
Test the connection using the custom shortcut

## 1️⃣ Generate SSH Key Pair 🔑
Run the following command to generate a new SSH key pair:
```
ssh-keygen
```

## 2️⃣ Copy the Public Key to the Remote Machine 📤
Use ssh-copy-id to install your public key on the remote server:

```
ssh-copy-id ivolve@172.25.250.10
```

## 3️⃣ Configure SSH Shortcut in ~/.ssh/config ⚙️
Edit or create your SSH config file:

```
vim ~/.ssh/config
```
Add the following entry:

```
Host ivolve
    HostName 172.25.250.10
    User ivolve
    IdentityFile ~/.ssh/id_rsa
```

## 4️⃣ Test the SSH Connection 🚀
Connect using your shortcut:

```
ssh ivolve
```
![Alt Text](./images/ssh5.jpg)